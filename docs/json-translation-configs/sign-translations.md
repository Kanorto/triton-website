---
description: "Complete reference for sign translation items in JSON configuration files"
sidebar_position: 4
---

# Sign Translations

## Introduction

Sign translations (also called **Sign Groups**) allow you to translate the text on in-game signs. Unlike text translations, sign translations work with specific block locations in the world.

## Basic Structure

```json
{
  "type": "sign",
  "key": "sign.welcome",
  "lines": {
    "en_GB": ["&6Welcome", "&fto the", "&bServer", "&aEnjoy!"],
    "pt_PT": ["&6Bem-vindo", "&fao", "&bServidor", "&aDivirta-se!"]
  },
  "locations": [
    {
      "world": "world",
      "x": 100,
      "y": 65,
      "z": 200
    }
  ]
}
```

## Required Fields

### `type`

**Type:** String  
**Value:** `"sign"`

Must always be set to `"sign"` for sign translations.

### `key`

**Type:** String

A unique identifier for this sign group. This key is used with the `/triton sign` command to manage signs.

```
/triton sign <key>
```

:::info
Unlike text translations, sign keys are **not** used in `[lang]` placeholders. They are only used for the sign management command.
:::

### `lines`

**Type:** JSON Object (keys are language IDs, values are string arrays)

Contains exactly **4 strings** per language, representing the 4 lines of a Minecraft sign (top to bottom).

```json
{
  "lines": {
    "en_GB": ["Line 1", "Line 2", "Line 3", "Line 4"],
    "pt_PT": ["Linha 1", "Linha 2", "Linha 3", "Linha 4"]
  }
}
```

:::warning
Each language **must** have exactly 4 strings in its array, even if some lines are empty. Use an empty string `""` for blank lines.
:::

**Example with empty lines:**

```json
{
  "lines": {
    "en_GB": ["&6Shop", "", "&aOpen!", ""],
    "pt_PT": ["&6Loja", "", "&aAberta!", ""]
  }
}
```

### `locations`

**Type:** JSON Array of Location objects

Defines which signs in the world belong to this sign group. Each location object specifies a sign's world and coordinates.

#### Spigot Location Format

```json
{
  "locations": [
    {
      "world": "world",
      "x": 100,
      "y": 65,
      "z": 200
    }
  ]
}
```

#### BungeeCord Location Format

When using BungeeCord, you must include the `server` field:

```json
{
  "locations": [
    {
      "server": "lobby",
      "world": "world",
      "x": 100,
      "y": 65,
      "z": 200
    }
  ]
}
```

### Location Fields

| Field    | Type    | Required       | Description                          |
|----------|---------|----------------|--------------------------------------|
| `world`  | String  | Always         | The world name where the sign exists |
| `x`      | Integer | Always         | The X coordinate of the sign         |
| `y`      | Integer | Always         | The Y coordinate of the sign         |
| `z`      | Integer | Always         | The Z coordinate of the sign         |
| `server` | String  | BungeeCord only | The server name where the sign exists |

## Multiple Signs in One Group

A single sign group can contain multiple signs that share the same translated text. This is useful for signs that appear in multiple locations but show the same content.

```json
{
  "type": "sign",
  "key": "sign.rules",
  "lines": {
    "en_GB": ["&c&lRules", "&f1. Be nice", "&f2. No griefing", "&f3. Have fun"],
    "pt_PT": ["&c&lRegras", "&f1. Seja gentil", "&f2. Sem griefing", "&f3. Divirta-se"]
  },
  "locations": [
    {
      "world": "world",
      "x": 100,
      "y": 65,
      "z": 200
    },
    {
      "world": "world",
      "x": 105,
      "y": 65,
      "z": 200
    },
    {
      "world": "world_nether",
      "x": 50,
      "y": 70,
      "z": 50
    }
  ]
}
```

## Color Codes on Signs

Signs support the same Minecraft color codes as text translations using the `&` symbol:

```json
{
  "lines": {
    "en_GB": ["&a&lGreen Bold", "&6Gold Text", "&b&oAqua Italic", "&cRed"]
  }
}
```

## Dynamic Signs

:::note[Requirements]
Requires Triton v2.3.0 or later.
:::

Some signs display dynamic information (e.g., player counts, game status). You can preserve these dynamic lines by using the special placeholder `%use_line_default%`.

When a line is set to `%use_line_default%`, Triton will **not** translate that line and instead show the original sign content, allowing other plugins to update it dynamically.

### Example: Game Join Sign

Suppose a minigame plugin creates a sign that looks like:

```
[SkyWars]
Game #3
Waiting
12/16
```

You want to translate the first line but keep the rest dynamic:

```json
{
  "type": "sign",
  "key": "sign.skywars.join",
  "lines": {
    "en_GB": ["&b[SkyWars]", "%use_line_default%", "%use_line_default%", "%use_line_default%"],
    "pt_PT": ["&b[GuerraCéu]", "%use_line_default%", "%use_line_default%", "%use_line_default%"]
  },
  "locations": [
    {
      "world": "lobby",
      "x": 10,
      "y": 64,
      "z": 20
    }
  ]
}
```

In this configuration:
- **Line 1** is translated (changes per player language).
- **Lines 2, 3, 4** remain untouched and show whatever the minigame plugin sets.

:::tip
You can combine dynamic signs with [patterns](../concepts/patterns.md) to translate the dynamic content itself (e.g., game statuses like "Waiting" → "Esperando").
:::

## Finding Sign Coordinates

To find the coordinates of a sign for the `locations` field:

1. Stand near the sign in-game.
2. Look at the sign and press **F3** to open the debug screen.
3. Note the **Block** coordinates shown in the debug screen.
4. Alternatively, use the `/triton sign` command, which can auto-detect the sign you're looking at.

## Complete Sign Translation Example

```json
{
  "type": "sign",
  "key": "sign.shop.entrance",
  "lines": {
    "en_GB": ["&6&l═══════", "&a&lSHOP", "&fClick to enter", "&6&l═══════"],
    "pt_PT": ["&6&l═══════", "&a&lLOJA", "&fClique para entrar", "&6&l═══════"],
    "es_ES": ["&6&l═══════", "&a&lTIENDA", "&fHaz clic para entrar", "&6&l═══════"]
  },
  "locations": [
    {
      "world": "world",
      "x": 150,
      "y": 64,
      "z": -30
    },
    {
      "world": "world",
      "x": 155,
      "y": 64,
      "z": -30
    }
  ]
}
```
