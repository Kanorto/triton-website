---
description: "Real-world examples, complete collection files, common mistakes, and validation tips"
sidebar_position: 6
---

# Examples & Best Practices

## Complete Collection File Examples

### Example 1: Simple Spigot Collection

A basic collection file for a standalone Spigot server with text and sign translations:

```json title="plugins/Triton/translations/default.json"
[
  {
    "type": "text",
    "key": "welcome.join",
    "languages": {
      "en_GB": "&6Welcome to the server, &e%1&6!",
      "pt_PT": "&6Bem-vindo ao servidor, &e%1&6!"
    }
  },
  {
    "type": "text",
    "key": "welcome.first_join",
    "languages": {
      "en_GB": "&a&l★ &e%1 &ajoined for the first time! &a&l★",
      "pt_PT": "&a&l★ &e%1 &aentrou pela primeira vez! &a&l★"
    }
  },
  {
    "type": "text",
    "key": "error.no_permission",
    "languages": {
      "en_GB": "&cYou don't have permission to do that!",
      "pt_PT": "&cVocê não tem permissão para fazer isso!"
    }
  },
  {
    "type": "sign",
    "key": "sign.spawn",
    "lines": {
      "en_GB": ["&6&l═══════", "&a&lSPAWN", "&fRight-click", "&6&l═══════"],
      "pt_PT": ["&6&l═══════", "&a&lINÍCIO", "&fClique-direito", "&6&l═══════"]
    },
    "locations": [
      {
        "world": "world",
        "x": 0,
        "y": 65,
        "z": 0
      }
    ]
  }
]
```

### Example 2: Plugin-Specific Collection

A dedicated collection for translating the CMI plugin:

```json title="plugins/Triton/translations/cmi.json"
[
  {
    "type": "text",
    "key": "cmi.info.NoPermission",
    "languages": {
      "en_GB": "&cYou don't have permission!",
      "pt_PT": "&cNão tens permissão!",
      "es_ES": "&c¡No tienes permiso!"
    }
  },
  {
    "type": "text",
    "key": "cmi.info.NoPlayerPermission",
    "languages": {
      "en_GB": "&c%1 doesn't have permission for: %2",
      "pt_PT": "&c%1 não tem permissão para: %2",
      "es_ES": "&c%1 no tiene permiso para: %2"
    }
  },
  {
    "type": "text",
    "key": "cmi.info.Ingame",
    "languages": {
      "en_GB": "&cYou can only use this in game!",
      "pt_PT": "&cSó podes usar isto no jogo!",
      "es_ES": "&c¡Solo puedes usar esto en el juego!"
    }
  },
  {
    "type": "text",
    "key": "cmi.info.NoInformation",
    "languages": {
      "en_GB": "&cNo information found!",
      "pt_PT": "&cNenhuma informação encontrada!",
      "es_ES": "&c¡No se encontró información!"
    }
  }
]
```

### Example 3: BungeeCord Network Collection

A collection for a BungeeCord network with server-specific messages:

```json title="plugins/Triton/translations/network.json"
{
  "metadata": {
    "blacklist": true,
    "servers": []
  },
  "items": [
    {
      "type": "text",
      "key": "network.motd",
      "languages": {
        "en_GB": "&6&lMy Network &8- &aOnline!",
        "pt_PT": "&6&lMinha Rede &8- &aOnline!"
      }
    },
    {
      "type": "text",
      "key": "lobby.welcome",
      "languages": {
        "en_GB": "&bWelcome to the &6&lLobby&b!",
        "pt_PT": "&bBem-vindo ao &6&lLobby&b!"
      },
      "servers": ["lobby-1", "lobby-2"],
      "blacklist": false
    },
    {
      "type": "text",
      "key": "survival.welcome",
      "languages": {
        "en_GB": "&2Welcome to &a&lSurvival&2! Good luck!",
        "pt_PT": "&2Bem-vindo ao &a&lSurvival&2! Boa sorte!"
      },
      "servers": ["survival"],
      "blacklist": false
    },
    {
      "type": "text",
      "key": "creative.welcome",
      "languages": {
        "en_GB": "&eWelcome to &6&lCreative&e! Build freely!",
        "pt_PT": "&eBem-vindo ao &6&lCreative&e! Construa à vontade!"
      },
      "servers": ["creative"],
      "blacklist": false
    }
  ]
}
```

### Example 4: Advanced Features Collection

A collection demonstrating patterns, JSON chat, and MiniMessage:

```json title="plugins/Triton/translations/advanced.json"
[
  {
    "type": "text",
    "key": "pattern.death.generic",
    "languages": {
      "en_GB": "&7%1 &fdied",
      "pt_PT": "&7%1 &fmorreu"
    },
    "patterns": [
      "^(\\w+) died$",
      "^(\\w+) was killed$"
    ]
  },
  {
    "type": "text",
    "key": "pattern.death.pvp",
    "languages": {
      "en_GB": "&c%1 &fwas slain by &c%2",
      "pt_PT": "&c%1 &ffoi morto por &c%2"
    },
    "patterns": [
      "^(\\w+) was slain by (\\w+)$"
    ]
  },
  {
    "type": "text",
    "key": "clickable.website",
    "languages": {
      "en_GB": "[triton_json]{\"text\":\"§6§l[Click] §fVisit our website!\",\"clickEvent\":{\"action\":\"open_url\",\"value\":\"https://example.com\"},\"hoverEvent\":{\"action\":\"show_text\",\"contents\":\"§eClick to open\"}}",
      "pt_PT": "[triton_json]{\"text\":\"§6§l[Clique] §fVisite nosso website!\",\"clickEvent\":{\"action\":\"open_url\",\"value\":\"https://example.com\"},\"hoverEvent\":{\"action\":\"show_text\",\"contents\":\"§eClique para abrir\"}}"
    }
  },
  {
    "type": "text",
    "key": "minimsg.announcement",
    "languages": {
      "en_GB": "[minimsg]<gradient:#ff0000:#0000ff><bold>SERVER ANNOUNCEMENT</bold></gradient><newline><yellow>%1",
      "pt_PT": "[minimsg]<gradient:#ff0000:#0000ff><bold>ANÚNCIO DO SERVIDOR</bold></gradient><newline><yellow>%1"
    }
  },
  {
    "type": "sign",
    "key": "sign.game.skywars",
    "lines": {
      "en_GB": ["&b&l[SkyWars]", "%use_line_default%", "%use_line_default%", "%use_line_default%"],
      "pt_PT": ["&b&l[GuerraCéu]", "%use_line_default%", "%use_line_default%", "%use_line_default%"]
    },
    "locations": [
      {
        "world": "lobby",
        "x": 10,
        "y": 64,
        "z": 20
      },
      {
        "world": "lobby",
        "x": 12,
        "y": 64,
        "z": 20
      }
    ]
  }
]
```

### Example 5: Multi-Language Collection

A collection supporting many languages:

```json title="plugins/Triton/translations/multilang.json"
[
  {
    "type": "text",
    "key": "menu.language.title",
    "languages": {
      "en_GB": "&6Select your language",
      "pt_PT": "&6Selecione seu idioma",
      "es_ES": "&6Selecciona tu idioma",
      "de_DE": "&6Wähle deine Sprache",
      "fr_FR": "&6Choisissez votre langue",
      "it_IT": "&6Scegli la tua lingua",
      "ru_RU": "&6Выберите ваш язык",
      "ja_JP": "&6言語を選択してください",
      "zh_CN": "&6选择你的语言",
      "ko_KR": "&6언어를 선택하세요"
    }
  },
  {
    "type": "text",
    "key": "menu.language.selected",
    "languages": {
      "en_GB": "&aLanguage changed to English!",
      "pt_PT": "&aIdioma alterado para Português!",
      "es_ES": "&a¡Idioma cambiado a Español!",
      "de_DE": "&aSprache auf Deutsch geändert!",
      "fr_FR": "&aLangue changée en Français !",
      "it_IT": "&aLingua cambiata in Italiano!",
      "ru_RU": "&aЯзык изменён на Русский!",
      "ja_JP": "&a言語が日本語に変更されました！",
      "zh_CN": "&a语言已更改为中文！",
      "ko_KR": "&a언어가 한국어로 변경되었습니다!"
    }
  }
]
```

## Best Practices

### Key Naming Convention

Use a consistent naming scheme for your translation keys:

```
<plugin>.<category>.<message>
```

**Examples:**
| Key | Description |
|-----|-------------|
| `cmi.info.NoPermission` | CMI's no permission message |
| `essentials.teleport.success` | Essentials teleport success |
| `shop.buy.confirm` | Shop purchase confirmation |
| `menu.main.title` | Main menu title |
| `sign.spawn.welcome` | Spawn welcome sign |

### One Collection Per Plugin

Keep translations organized by creating one collection file per plugin:

```
translations/
├── default.json       ← generic server messages
├── essentials.json    ← Essentials translations
├── cmi.json           ← CMI translations
├── shops.json         ← Shop plugin translations
└── signs.json         ← All sign translations
```

### Always Include the Main Language

Always include a translation for your main language (defined in `config.yml`). This serves as the fallback when a translation is missing in the player's language:

```json
{
  "type": "text",
  "key": "example",
  "languages": {
    "en_GB": "This is always needed if en_GB is your main language",
    "pt_PT": "Optional: only if you support Portuguese"
  }
}
```

### Use Color Codes Consistently

Maintain a consistent color scheme across your translations:

```json
{
  "languages": {
    "en_GB": "&c[Error] &fSomething went wrong!",
    "pt_PT": "&c[Erro] &fAlgo deu errado!"
  }
}
```

Note that the color codes (`&c`, `&f`) remain the same across all languages to maintain visual consistency.

### Keep Keys Unique Across All Collections

Translation keys must be unique across **all** collection files. If two items share the same key, only one will be used and behavior becomes unpredictable.

### Validate JSON Before Saving

Always validate your JSON files before deploying them to the server. Use tools like:

- [JSONLint](https://jsonlint.com/)
- [JSON Formatter](https://jsonformatter.org/json-pretty-print)
- [Visual Studio Code](https://code.visualstudio.com) with built-in JSON validation

## Common Mistakes

### ❌ Trailing Comma

```json
[
  {
    "type": "text",
    "key": "msg1",
    "languages": { "en_GB": "Hello" }
  },  // ← ERROR: trailing comma after last item
]
```

**Fix:** Remove the comma after the last item.

```json
[
  {
    "type": "text",
    "key": "msg1",
    "languages": { "en_GB": "Hello" }
  }
]
```

### ❌ Missing `type` Field

```json
{
  "key": "welcome",
  "languages": { "en_GB": "Welcome!" }
}
```

**Fix:** Always include `"type": "text"` or `"type": "sign"`.

```json
{
  "type": "text",
  "key": "welcome",
  "languages": { "en_GB": "Welcome!" }
}
```

### ❌ Using `languages` for Signs

```json
{
  "type": "sign",
  "key": "sign.0",
  "languages": {
    "en_GB": "Line 1"
  }
}
```

**Fix:** Signs use `lines` (with arrays of 4 strings), not `languages`.

```json
{
  "type": "sign",
  "key": "sign.0",
  "lines": {
    "en_GB": ["Line 1", "Line 2", "Line 3", "Line 4"]
  },
  "locations": [{ "world": "world", "x": 0, "y": 64, "z": 0 }]
}
```

### ❌ Wrong Number of Sign Lines

```json
{
  "type": "sign",
  "key": "sign.0",
  "lines": {
    "en_GB": ["Line 1", "Line 2"]
  }
}
```

**Fix:** Always provide exactly 4 lines per language.

```json
{
  "type": "sign",
  "key": "sign.0",
  "lines": {
    "en_GB": ["Line 1", "Line 2", "", ""]
  }
}
```

### ❌ Missing Sign Locations

```json
{
  "type": "sign",
  "key": "sign.0",
  "lines": {
    "en_GB": ["Line 1", "Line 2", "Line 3", "Line 4"]
  }
}
```

**Fix:** Signs require the `locations` field to know which blocks to translate.

```json
{
  "type": "sign",
  "key": "sign.0",
  "lines": {
    "en_GB": ["Line 1", "Line 2", "Line 3", "Line 4"]
  },
  "locations": [
    { "world": "world", "x": 100, "y": 65, "z": 200 }
  ]
}
```

### ❌ Mismatched Language IDs

```json
{
  "type": "text",
  "key": "test",
  "languages": {
    "english": "Hello!",
    "portuguese": "Olá!"
  }
}
```

**Fix:** Language IDs must match those defined in `config.yml`.

```json
{
  "type": "text",
  "key": "test",
  "languages": {
    "en_GB": "Hello!",
    "pt_PT": "Olá!"
  }
}
```

### ❌ Unescaped Special Characters in Patterns

```json
{
  "patterns": [
    "Player (someone) joined."
  ]
}
```

**Fix:** Escape regex special characters.

```json
{
  "patterns": [
    "Player \\(someone\\) joined\\."
  ]
}
```

### ❌ Duplicate Keys

Having two items with the same key in different files causes unpredictable behavior:

```json title="chat.json"
[{ "type": "text", "key": "welcome", "languages": { "en_GB": "Hi!" } }]
```

```json title="greetings.json"
[{ "type": "text", "key": "welcome", "languages": { "en_GB": "Hello!" } }]
```

**Fix:** Use unique keys across all collection files. For example: `chat.welcome` and `greetings.welcome`.

## Workflow Checklist

When creating translations manually, follow this checklist:

1. ✅ Create or choose a collection file in `translations/`
2. ✅ Add the translation item with all required fields (`type`, `key`, `languages`/`lines`)
3. ✅ Use language IDs matching your `config.yml`
4. ✅ Include the main language translation as a fallback
5. ✅ Replace variables with `%1`, `%2`, etc.
6. ✅ Validate JSON syntax
7. ✅ Reload with `/triton reload`
8. ✅ Replace original plugin messages with `[lang]key[/lang]` placeholders
9. ✅ Test in-game with different languages
