---
description: "Complete overview of Triton's JSON translation configuration system"
sidebar_position: 1
---

# JSON Translation Configs Overview

## Introduction

Triton stores all translations in JSON files located in the `translations/` folder inside the `plugins/Triton` directory. Each JSON file represents a **collection** — a logical group of translations that can be managed independently.

This documentation section provides a comprehensive reference for creating, structuring, and managing these JSON configuration files manually. If you prefer a visual editor, consider using [TWIN](../concepts/twin.md) instead.

## File Location

```
plugins/
└── Triton/
    ├── config.yml
    └── translations/
        ├── default.json      ← default collection (auto-created)
        ├── chat.json          ← custom collection
        ├── signs.json         ← custom collection
        └── ...
```

:::tip
When using BungeeCord, the `translations/` folder is only loaded from the **proxy server**. Translation files on individual Spigot servers are ignored.
:::

## Key Concepts

### Collections

A **collection** is a single JSON file in the `translations/` folder. Collections let you organize translations logically — for example, by plugin, feature, or language group.

- Triton automatically creates a `default.json` collection on first run.
- You can create as many collections as you need.
- Collection names are derived from the file name (e.g., `chat.json` → collection `chat`).

### Translatable Items

Each collection contains **translatable items** — individual translation entries. There are two types:

| Type   | Purpose                            | Key Fields                          |
|--------|------------------------------------|-------------------------------------|
| `text` | Chat messages, titles, items, etc. | `type`, `key`, `languages`          |
| `sign` | In-game signs                      | `type`, `key`, `lines`, `locations` |

### Language IDs

Language IDs are the identifiers you define in your `config.yml` under the `languages` section. Common examples include `en_GB`, `pt_PT`, `es_ES`, `de_DE`, etc. These same IDs are used as keys in the `languages` and `lines` fields of your translation items.

## File Formats

Triton supports two JSON file formats depending on your server setup:

### Spigot Format (Simple Array)

For standalone Spigot servers, the collection file is a simple JSON array:

```json
[
  {
    "type": "text",
    "key": "welcome.message",
    "languages": {
      "en_GB": "Welcome to the server!",
      "pt_PT": "Bem-vindo ao servidor!"
    }
  }
]
```

### BungeeCord Format (With Metadata)

For BungeeCord networks, you can use an extended format that includes server filtering metadata:

```json
{
  "metadata": {
    "blacklist": true,
    "servers": []
  },
  "items": [
    {
      "type": "text",
      "key": "welcome.message",
      "languages": {
        "en_GB": "Welcome to the server!",
        "pt_PT": "Bem-vindo ao servidor!"
      }
    }
  ]
}
```

:::info
Both formats work on BungeeCord. The extended format with `metadata` simply allows you to apply server filtering rules to an entire collection at once.
:::

## Documentation Structure

This documentation section is organized as follows:

- **[Collection Structure](./collection-structure.md)** — How to create and organize collection files
- **[Text Translations](./text-translations.md)** — Complete reference for text translation items
- **[Sign Translations](./sign-translations.md)** — Complete reference for sign translation items
- **[Advanced Features](./advanced-features.md)** — Patterns, server filtering, and special formatting
- **[Examples & Best Practices](./examples.md)** — Real-world examples, common mistakes, and validation tips

## Quick Start

Here is the simplest possible translation file you can create:

**File:** `plugins/Triton/translations/default.json`

```json
[
  {
    "type": "text",
    "key": "hello",
    "languages": {
      "en_GB": "Hello!",
      "pt_PT": "Olá!"
    }
  }
]
```

After creating this file, run `/triton reload` in-game or from the console to load the translations.

To use this translation in a plugin config, replace the original message with a [Triton placeholder](../concepts/placeholders.md):

```
[lang]hello[/lang]
```
