# FS25 Modding Referenz

Eine umfassende deutsche Referenz für die KI-gestützte Entwicklung von Mods für **Farming Simulator 25**.

Dieses Repository dient als Wissensbasis für Large Language Models (z. B. Claude, ChatGPT) und Entwickler, die Mods für FS25 erstellen oder warten. Alle Inhalte sind auf die Verwendung als Kontext für KI-Programmierungsassistenten optimiert – kompakt, strukturiert und direkt verwendbar.

---

## Inhalt

Die Hauptdatei [`FS25_LUA_REFERENZ.md`](./FS25_LUA_REFERENZ.md) deckt folgende Themen ab:

| # | Thema |
|---|---|
| 1 | Quellen & Tools (21 Ressourcen inkl. GDN, GitHub-Repos, Bücher, Foren) |
| 2 | Mod-Ordnerstruktur |
| 3 | modDesc.xml (Fahrzeuge, Placeables, Specializations) |
| 4 | Lua Klassen-Pattern (OOP mit Metatables) |
| 5 | Vehicle Specialization (vollständige Registrierung + Lifecycle) |
| 6 | Placeable Specialization |
| 7 | Event-Listener Übersicht |
| 8 | Netzwerk & Multiplayer (Streams, DirtyFlags) |
| 9 | XML Schema registrieren (XMLValueType, Arrays, Savegame) |
| 10–15 | Engine API: Node, Math/Bit, HUD/Rendering, Sound, Input, Allgemein |
| 16–20 | Script API: SpecializationUtil, MathUtil, Vehicle, Motorized, Drivable |
| 21 | Trigger-System |
| 22 | i18n Übersetzungen |
| 23 | Bekannte Fallstricke |
| 24 | Komplettes Spec-Grundgerüst (copy-paste Template) |
| 25 | Map Modding – Ordnerstruktur, map.xml, Terrain, Tools |
| 26 | Production Chains – XML, Inputs/Outputs, Lua-Abfragen |
| 27 | ModHub Einreichung – Namenskonventionen, Icons, Checkliste |
| 28 | Referenz-Mods auf GitHub + VSCode-Setup |
| 29 | Mod Sync Server API (Pre-Join HTTP API) |

---

## Verwendung mit KI

Die Referenz ist so aufgebaut, dass sie direkt als Kontext in KI-Assistenten eingefügt werden kann:

- **Claude / ChatGPT / Copilot:** Dateiinhalt als System-Prompt oder Kontext einfügen
- **Claude Code:** Repository als Arbeitsverzeichnis öffnen – der Assistent liest die Referenz automatisch
- **Cursor / Windsurf:** Datei in den Kontext ziehen und auf die Abschnitte verweisen

**Beispiel-Prompt:**
```
Verwende die FS25_LUA_REFERENZ.md als Grundlage.
Erstelle eine Vehicle Specialization, die den Kraftstoffverbrauch
auf Basis der Motor-RPM dynamisch anpasst und per HUD anzeigt.
```

---

## FS25 Version

Getestet und dokumentiert für **Farming Simulator 25 v1.15.0.0**.

---

## Quellen

Die Referenz aggregiert Informationen aus offiziellen und Community-Quellen:

- [GIANTS Developer Network (GDN)](https://gdn.giants-software.com/)
- [Community LuaDoc](https://umbraprior.github.io/FS25-Community-LUADOC/)
- [FS25 Lua Scripting – dataS Dump](https://github.com/Dukefarming/FS25-lua-scripting)
- [FS25 Lua API Stubs](https://github.com/MyGameSteamOfficial/fs25-lua-api)
- [Scripting Farming Simulator with Lua (Open Access)](https://library.oapen.org/handle/20.500.12657/86976)
- [maps4fs](https://maps4fs.xyz)
- [FS25 Mod Sync Server](https://github.com/spliffz/FS25-Mod-Sync-Server)

---

## Lizenz

Inhalte unter [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) – freie Verwendung mit Namensnennung.
