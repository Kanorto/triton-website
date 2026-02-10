---
description: "How to create, structure, and manage translation collection files"
sidebar_position: 2
---

# Collection Structure

## What Is a Collection?

A collection is a JSON file inside the `translations/` folder that groups multiple translatable items together. Think of it as a folder of related translations.

## Creating a Collection

To create a new collection, simply create a new `.json` file inside the `translations/` folder.

### Empty Collection (Spigot)

```json
[]
```

### Empty Collection (BungeeCord with Metadata)

```json
{
  "metadata": {
    "blacklist": true,
    "servers": []
  },
  "items": []
}
```

## Spigot Format

On standalone Spigot servers, a collection is a JSON **array** of translatable items:

```json
[
  {
    "type": "text",
    "key": "first.translation",
    "languages": {
      "en_GB": "Hello!",
      "pt_PT": "Olá!"
    }
  },
  {
    "type": "text",
    "key": "second.translation",
    "languages": {
      "en_GB": "Goodbye!",
      "pt_PT": "Adeus!"
    }
  }
]
```

### Rules

- The root element **must** be a JSON array (`[...]`).
- Each element in the array is a translatable item (either `text` or `sign`).
- You can mix `text` and `sign` items in the same collection.

## BungeeCord Format

On BungeeCord networks, you can optionally use an extended format with a `metadata` section:

```json
{
  "metadata": {
    "blacklist": true,
    "servers": ["lobby-1", "lobby-2"]
  },
  "items": [
    {
      "type": "text",
      "key": "lobby.welcome",
      "languages": {
        "en_GB": "Welcome to the lobby!",
        "pt_PT": "Bem-vindo ao lobby!"
      }
    }
  ]
}
```

### Metadata Fields

| Field       | Type          | Default | Description                                                                 |
|-------------|---------------|---------|-----------------------------------------------------------------------------|
| `blacklist` | Boolean       | `true`  | If `true`, `servers` acts as a blacklist. If `false`, it acts as a whitelist. |
| `servers`   | String array  | `[]`    | List of server names to include/exclude.                                     |

### How Server Filtering Works

- **Blacklist mode** (`"blacklist": true`): Translations are available on **all** servers **except** those listed in `servers`.
- **Whitelist mode** (`"blacklist": false`): Translations are available **only** on the servers listed in `servers`.
- **Empty `servers` array with blacklist mode**: Translations are available on all servers (no servers are excluded).

:::tip
You can also set `servers` and `blacklist` on **individual text items** to get per-item server filtering. See [Advanced Features](./advanced-features.md#server-filtering-bungeecord) for details.
:::

## Organizing Collections

There is no strict naming convention, but here are some recommended approaches:

### By Plugin

```
translations/
├── default.json      ← general translations
├── essentials.json   ← EssentialsX translations
├── cmi.json          ← CMI translations
└── worldguard.json   ← WorldGuard translations
```

### By Feature

```
translations/
├── chat.json         ← chat messages
├── menus.json        ← GUI menus
├── signs.json        ← sign translations
└── items.json        ← item names and lores
```

### By Server (BungeeCord)

```
translations/
├── global.json       ← shared across all servers
├── lobby.json        ← lobby-specific (use metadata filtering)
├── survival.json     ← survival-specific
└── minigames.json    ← minigames-specific
```

## Deleting a Collection

To delete a collection, simply delete the corresponding `.json` file from the `translations/` folder.

:::warning
Deleting a collection file permanently removes **all** translations inside it. Copy any translations you want to keep to another collection file before deleting.
:::

## Moving Translations Between Collections

To move a translation from one collection to another:

1. Open the source collection file.
2. Cut the translation item (the entire JSON object).
3. Open the destination collection file.
4. Paste the translation item into the `items` array (or root array for Spigot format).
5. Save both files.
6. Run `/triton reload`.

## Validation

Always ensure your JSON files are valid before saving. Invalid JSON will cause Triton to fail loading the collection.

:::tip
Use a JSON validator tool:
- [JSONLint](https://jsonlint.com/)
- [JSON Formatter](https://jsonformatter.org/json-pretty-print)
- Or use an IDE like [Visual Studio Code](https://code.visualstudio.com) which highlights JSON errors automatically.
:::

### Common JSON Errors

| Error | Cause | Fix |
|-------|-------|-----|
| Unexpected token | Missing comma between items | Add `,` between objects in the array |
| Unexpected end of input | Missing closing bracket | Ensure all `[`, `{` have matching `]`, `}` |
| Duplicate key | Same `key` value in the same file | Use unique keys for each translation |
| Trailing comma | Extra `,` after the last item | Remove the trailing comma |
