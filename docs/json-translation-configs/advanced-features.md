---
description: "Advanced features for JSON translation configs including patterns, server filtering, and special formatting"
sidebar_position: 5
---

# Advanced Features

## Patterns

:::note[Requirements]
Requires Triton v2.0.0 or later.
:::

Patterns allow you to translate messages **without** editing the original plugin configuration. This is useful when a plugin doesn't allow you to edit its messages or when you want to automatically intercept and replace messages.

Each pattern is a [regular expression](https://regexr.com/) (regex) that is checked against every message. When a match is found, the message is replaced with the corresponding translation.

### Basic Pattern Usage

Add the `patterns` field to any text translation item:

```json
{
  "type": "text",
  "key": "no.permission",
  "languages": {
    "en_GB": "&cYou don't have permission!",
    "pt_PT": "&cVocê não tem permissão!"
  },
  "patterns": [
    "&cYou do not have access to that command\\."
  ]
}
```

:::warning[Performance]
Each pattern is checked against **every** message sent to players. Use patterns only when you cannot use [Triton placeholders](../concepts/placeholders.md) instead. Having too many patterns can impact server performance.
:::

### Escaping Special Characters

Since patterns are regex, special characters must be escaped with a backslash (`\`). In JSON, backslashes themselves need to be escaped, so you use double backslashes (`\\`).

| Character | Regex Escape | JSON String |
|-----------|-------------|-------------|
| `.`       | `\.`        | `\\.`       |
| `(`       | `\(`        | `\\(`       |
| `)`       | `\)`        | `\\)`       |
| `[`       | `\[`        | `\\[`       |
| `]`       | `\]`        | `\\]`       |
| `+`       | `\+`        | `\\+`       |
| `*`       | `\*`        | `\\*`       |
| `?`       | `\?`        | `\\?`       |
| `{`       | `\{`        | `\\{`       |
| `}`       | `\}`        | `\\}`       |
| `^`       | `\^`        | `\\^`       |
| `$`       | `\$`        | `\\$`       |
| `\|`      | `\\|`       | `\\\\|`     |

### Using Anchors

To prevent patterns from matching text typed by players in chat, use the beginning (`^`) and end (`$`) anchors:

```json
{
  "patterns": [
    "^&cYou do not have access to that command\\.$"
  ]
}
```

This ensures the pattern only matches the **exact** message, not a substring within a chat message.

### Patterns with Variables

You can capture dynamic parts of a message using regex groups and use them as variables in the translation:

```json
{
  "type": "text",
  "key": "pvp.kill.pattern",
  "languages": {
    "en_GB": "&c%1 &fwas slain by &c%2",
    "pt_PT": "&c%1 &ffoi morto por &c%2"
  },
  "patterns": [
    "^(\\w+) was killed by (\\w+)$"
  ]
}
```

In this example:
- `(\\w+)` captures one or more word characters.
- The first capture group becomes `%1`, the second becomes `%2`.

### Multiple Patterns

A single translation item can have multiple patterns. If **any** pattern matches, the message is replaced:

```json
{
  "type": "text",
  "key": "no.permission.generic",
  "languages": {
    "en_GB": "&cYou don't have permission!",
    "pt_PT": "&cVocê não tem permissão!"
  },
  "patterns": [
    "^&cYou do not have access to that command\\.$",
    "^&cInsufficient permissions\\.$",
    "^&cNo permission\\.$"
  ]
}
```

### Color Code Variants

Depending on the plugin, messages might use `&` or `§` for color codes. You may need to try both:

```json
{
  "patterns": [
    "&cNo permission!",
    "§cNo permission!"
  ]
}
```

:::tip
You can use a regex alternation to match both: `[&§]cNo permission!`
:::

## Server Filtering (BungeeCord)

:::note
This feature is only available on BungeeCord (or Velocity) networks.
:::

You can restrict individual text translation items to specific servers using the `servers` and `blacklist` fields.

### Whitelist Mode

Show this translation **only** on the listed servers:

```json
{
  "type": "text",
  "key": "lobby.welcome",
  "languages": {
    "en_GB": "&6Welcome to the lobby!",
    "pt_PT": "&6Bem-vindo ao lobby!"
  },
  "servers": ["lobby-1", "lobby-2"],
  "blacklist": false
}
```

### Blacklist Mode

Show this translation on **all servers except** the listed ones:

```json
{
  "type": "text",
  "key": "survival.tip",
  "languages": {
    "en_GB": "&eTip: Watch out for creepers!",
    "pt_PT": "&eDica: Cuidado com os creepers!"
  },
  "servers": ["creative", "lobby"],
  "blacklist": true
}
```

### Default Behavior

| Field       | Default | Effect                           |
|-------------|---------|----------------------------------|
| `servers`   | `[]`    | No filtering (available everywhere) |
| `blacklist` | `true`  | Empty blacklist = available everywhere |

:::tip
Collection-level metadata filtering and item-level filtering work independently. If a collection's metadata excludes a server, individual items within that collection won't be available on that server regardless of their own `servers` setting.
:::

## Formatting Prefixes Summary

Triton supports special prefixes that change how the translation text is processed:

| Prefix          | Version  | Description                                      |
|-----------------|----------|--------------------------------------------------|
| `[triton_json]` | v3.1.0+  | Treats the text as a Minecraft JSON chat component |
| `[minimsg]`     | v3.5.1+  | Treats the text as Kyori's MiniMessage format (PaperMC only) |

These prefixes are placed at the **beginning** of the translation value (inside the `languages` object):

```json
{
  "languages": {
    "en_GB": "[triton_json]{\"text\":\"Hello!\",\"bold\":true}",
    "pt_PT": "[minimsg]<bold>Olá!</bold>"
  }
}
```

:::info
You can use different formatting prefixes for different languages within the same translation item. However, this is uncommon and generally not recommended for consistency.
:::

## Disabled Line Feature

The `disabled-line` feature (configured in `config.yml`) allows you to prevent specific messages from being sent to the player. When a message contains the disabled line placeholder, the behavior varies by context.

To use this, set up a translation with a specific key and use it as a placeholder where you want to suppress output:

```json
{
  "type": "text",
  "key": "disabled.line",
  "languages": {
    "en_GB": "",
    "pt_PT": ""
  }
}
```

Then in `config.yml`:
```yaml
disabled-line: 'disabled.line'
```

Any message containing `[lang]disabled.line[/lang]` will be suppressed. The behavior depends on the context:

- **Chat**: Message is not sent at all
- **Action bar**: Not displayed
- **Item titles**: Shows default material name
- **Item lores**: The line is removed
- **Books**: The page is deleted
- **Titles/subtitles**: Not displayed
- **Tab header/footer**: Removed
- **Bossbars**: Text becomes blank
- **GUI titles**: Title becomes blank
- **Entities**: Display name is hidden

## All Available Fields Reference

Here is a complete reference of all fields available for translatable items:

### Text Translation Fields

| Field       | Required | Type            | Default | Description |
|-------------|----------|-----------------|---------|-------------|
| `type`      | ✅       | `"text"`         | —       | Item type identifier |
| `key`       | ✅       | String          | —       | Unique translation key |
| `languages` | ✅       | Object          | —       | Translation strings per language |
| `patterns`  | ❌       | String array    | `[]`    | Regex patterns for auto-matching |
| `servers`   | ❌       | String array    | `[]`    | Server names for filtering (BungeeCord) |
| `blacklist` | ❌       | Boolean         | `true`  | Server list mode (BungeeCord) |

### Sign Translation Fields

| Field       | Required | Type            | Default | Description |
|-------------|----------|-----------------|---------|-------------|
| `type`      | ✅       | `"sign"`         | —       | Item type identifier |
| `key`       | ✅       | String          | —       | Unique sign group key |
| `lines`     | ✅       | Object          | —       | Sign lines per language (4 strings each) |
| `locations` | ✅       | Array           | —       | Sign block coordinates |
