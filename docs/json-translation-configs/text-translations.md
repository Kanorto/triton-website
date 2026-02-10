---
description: "Complete reference for text translation items in JSON configuration files"
sidebar_position: 3
---

# Text Translations

## Basic Structure

A text translation item represents a single translatable message. It is the most commonly used translation type.

```json
{
  "type": "text",
  "key": "my.translation.key",
  "languages": {
    "en_GB": "English text here",
    "pt_PT": "Portuguese text here"
  }
}
```

## Required Fields

### `type`

**Type:** String  
**Value:** `"text"`

Must always be set to `"text"` for text translations.

### `key`

**Type:** String

A unique identifier for this translation. This key is used in [Triton placeholders](../concepts/placeholders.md) to reference this translation.

```
[lang]my.translation.key[/lang]
```

**Key naming conventions:**

- Use dot-separated hierarchical names: `plugin.category.message`
- Keep keys descriptive but concise
- Use lowercase letters, numbers, dots, and underscores
- Avoid spaces and special characters

**Good examples:**
- `cmi.info.NoPermission`
- `essentials.teleport.success`
- `shop.buy.confirm`
- `death.killed_by_player`

**Bad examples:**
- `msg1` (not descriptive)
- `My Translation Key` (spaces and uppercase)
- `a` (too short and meaningless)

### `languages`

**Type:** JSON Object (keys are language IDs, values are strings)

Contains the actual translation text for each language. The keys must match the language IDs defined in your `config.yml`.

```json
{
  "languages": {
    "en_GB": "Welcome, adventurer!",
    "pt_PT": "Bem-vindo, aventureiro!",
    "es_ES": "¡Bienvenido, aventurero!",
    "de_DE": "Willkommen, Abenteurer!"
  }
}
```

:::warning
If a language ID used here doesn't exist in your `config.yml`, it will be ignored. Similarly, if a player's language is not present in the `languages` object, Triton will fall back to the main language (defined in `config.yml` under `main-language`) or follow the `fallback-languages` chain.
:::

## Color Codes & Formatting

Triton supports Minecraft color codes using the `&` symbol:

| Code | Color         | Code | Format        |
|------|---------------|------|---------------|
| `&0` | Black         | `&l` | **Bold**      |
| `&1` | Dark Blue     | `&m` | ~~Strikethrough~~ |
| `&2` | Dark Green    | `&n` | <u>Underline</u> |
| `&3` | Dark Aqua     | `&o` | *Italic*      |
| `&4` | Dark Red      | `&k` | Obfuscated    |
| `&5` | Dark Purple   | `&r` | Reset         |
| `&6` | Gold          |      |               |
| `&7` | Gray          |      |               |
| `&8` | Dark Gray     |      |               |
| `&9` | Blue          |      |               |
| `&a` | Green         |      |               |
| `&b` | Aqua          |      |               |
| `&c` | Red           |      |               |
| `&d` | Light Purple  |      |               |
| `&e` | Yellow        |      |               |
| `&f` | White         |      |               |

**Example:**

```json
{
  "type": "text",
  "key": "welcome.colored",
  "languages": {
    "en_GB": "&6&lWelcome &eto the server!",
    "pt_PT": "&6&lBem-vindo &eao servidor!"
  }
}
```

## Using Variables

Variables allow you to insert dynamic values into translations. Variables are defined with `%1`, `%2`, `%3`, etc.

```json
{
  "type": "text",
  "key": "death.kill",
  "languages": {
    "en_GB": "&c%1 &fhas killed &c%2",
    "pt_PT": "&c%1 &fmatou &c%2"
  }
}
```

Variables are passed through the [Triton placeholder](../concepts/placeholders.md) `[arg]` tags:

```
[lang]death.kill[args][arg]Player1[/arg][arg]Player2[/arg][/args][/lang]
```

In this example:
- `%1` → `Player1`
- `%2` → `Player2`

### Repeating Variables

You can use the same variable multiple times in a single translation:

```json
{
  "type": "text",
  "key": "pvp.announce",
  "languages": {
    "en_GB": "&c%1 &fkilled &c%2&f! &c%1 &fis on a streak!",
    "pt_PT": "&c%1 &fmatou &c%2&f! &c%1 &festá em série!"
  }
}
```

### Using Plugin Variables Inside `[arg]`

When configuring a third-party plugin, you can pass that plugin's variables inside the `[arg]` tags:

```
[lang]example.message[args][arg]%player_name%[/arg][arg]%player_health%[/arg][/args][/lang]
```

## JSON Chat Components

:::note[Requirements]
Requires Triton v3.1.0 or newer.
:::

Minecraft supports rich text through JSON chat components, enabling click events, hover events, and native Minecraft translations. To use JSON chat components, prepend `[triton_json]` to the translation value.

```json
{
  "type": "text",
  "key": "click.example",
  "languages": {
    "en_GB": "[triton_json]{\"text\":\"Click here!\",\"color\":\"gold\",\"bold\":true,\"clickEvent\":{\"action\":\"open_url\",\"value\":\"https://example.com\"},\"hoverEvent\":{\"action\":\"show_text\",\"contents\":\"Open website\"}}",
    "pt_PT": "[triton_json]{\"text\":\"Clique aqui!\",\"color\":\"gold\",\"bold\":true,\"clickEvent\":{\"action\":\"open_url\",\"value\":\"https://example.com\"},\"hoverEvent\":{\"action\":\"show_text\",\"contents\":\"Abrir website\"}}"
  }
}
```

### Using Minecraft's Native Translations with JSON

You can reference Minecraft's own localization keys:

```json
{
  "type": "text",
  "key": "gamemode.change",
  "languages": {
    "en_GB": "[triton_json]{\"translate\":\"gameMode.changed\",\"color\":\"gold\",\"bold\":true,\"with\":[{\"text\":\"%1\"}]}"
  }
}
```

:::warning
Variables inside JSON chat components (`%1`, `%2`, etc.) are **not** automatically escaped. Ensure that the variable values won't break the JSON structure.
:::

## Kyori's MiniMessage

:::note[Requirements]
Requires Triton v3.5.1 or newer and PaperMC (or a fork).
:::

[MiniMessage](https://docs.adventure.kyori.net/minimessage.html#format) is a simpler alternative to JSON chat components that supports HEX colors, click/hover events, and more. Prepend `[minimsg]` to use it.

```json
{
  "type": "text",
  "key": "pvp.kill.minimessage",
  "languages": {
    "en_GB": "[minimsg]<#006fff>PVP: <gold>%1</gold> has been killed by <#6100e9>%2",
    "pt_PT": "[minimsg]<#006fff>PVP: <gold>%1</gold> foi morto por <#6100e9>%2"
  }
}
```

### MiniMessage Advantages

- Easier HEX color syntax: `<#ff5555>` instead of complex JSON
- Simpler click/hover events: `<click:open_url:'https://example.com'>Click</click>`
- Gradient support: `<gradient:red:blue>Gradient text</gradient>`
- More readable than raw JSON

## Using PlaceholderAPI

:::note[Requirements]
Requires Triton v3.7.0 or newer.
:::

You can use any [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/) placeholder directly inside translation values. The placeholders are resolved per-player.

```json
{
  "type": "text",
  "key": "welcome.personalized",
  "languages": {
    "en_GB": "&6Welcome, &e%player_name%&6! Your balance: &a%vault_eco_balance%",
    "pt_PT": "&6Bem-vindo, &e%player_name%&6! Seu saldo: &a%vault_eco_balance%"
  }
}
```

:::info
PlaceholderAPI placeholders (e.g., `%player_name%`) are different from Triton variables (`%1`, `%2`). PlaceholderAPI placeholders are resolved automatically by the server; Triton variables are passed through `[arg]` tags in the placeholder syntax.
:::

## Complete Text Translation Item Example

Here is a comprehensive example combining multiple features:

```json
{
  "type": "text",
  "key": "shop.purchase.success",
  "languages": {
    "en_GB": "&a✔ &fYou bought &e%1x %2 &ffor &6%3 coins&f. Balance: &a%vault_eco_balance%",
    "pt_PT": "&a✔ &fCompraste &e%1x %2 &fpor &6%3 moedas&f. Saldo: &a%vault_eco_balance%",
    "es_ES": "&a✔ &fCompraste &e%1x %2 &fpor &6%3 monedas&f. Saldo: &a%vault_eco_balance%"
  },
  "patterns": [],
  "servers": ["survival-1", "survival-2"],
  "blacklist": false
}
```

This translation:
- Uses three Triton variables (`%1`, `%2`, `%3`) for quantity, item, and price
- Uses a PlaceholderAPI placeholder (`%vault_eco_balance%`)
- Uses color codes for formatting
- Is restricted to `survival-1` and `survival-2` servers (whitelist mode)
