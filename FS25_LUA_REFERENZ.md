# FS25 Lua Scripting – Universelle Referenz

> **Erstellt am:** 2026-05-27  
> **FS25 Version:** 1.15.0.0  
> **Quellen:** GDN LUADOC · github/Dukefarming/FS25-lua-scripting · github/MyGameSteamOfficial/fs25-lua-api · Buch „Scripting Farming Simulator with Lua" (Brumbaugh/Leithner, Apress) · Community LuaDoc (umbraprior.github.io)

---

## Inhaltsverzeichnis

1. [Quellen & Tools](#1-quellen--tools)
2. [Mod-Ordnerstruktur](#2-mod-ordnerstruktur)
3. [modDesc.xml](#3-moddescxml)
4. [Lua Klassen-Pattern](#4-lua-klassen-pattern)
5. [Vehicle Specialization](#5-vehicle-specialization)
6. [Placeable Specialization](#6-placeable-specialization)
7. [Wichtige Event-Listener](#7-wichtige-event-listener)
8. [Netzwerk & Multiplayer](#8-netzwerk--multiplayer)
9. [XML Schema registrieren](#9-xml-schema-registrieren)
10. [Engine API – Node](#10-engine-api--node)
11. [Engine API – Math & Bit](#11-engine-api--math--bit)
12. [Engine API – HUD & Rendering](#12-engine-api--hud--rendering)
13. [Engine API – Sound](#13-engine-api--sound)
14. [Engine API – Input](#14-engine-api--input)
15. [Engine API – Allgemein (print, XML)](#15-engine-api--allgemein-print-xml)
16. [Script API – SpecializationUtil](#16-script-api--specializationutil)
17. [Script API – MathUtil](#17-script-api--mathutil)
18. [Script API – Vehicle-Klasse](#18-script-api--vehicle-klasse)
19. [Script API – Motorized Spec](#19-script-api--motorized-spec)
20. [Script API – Drivable Spec](#20-script-api--drivable-spec)
21. [Trigger-System](#21-trigger-system)
22. [i18n Übersetzungen](#22-i18n-übersetzungen)
23. [Bekannte Fallstricke](#23-bekannte-fallstricke)
24. [Komplettes Spec-Grundgerüst](#24-komplettes-spec-grundgerüst)
25. [Map Modding – Grundstruktur](#25-map-modding--grundstruktur)
26. [Production Chains](#26-production-chains)
27. [ModHub Einreichung](#27-modhub-einreichung)
28. [Referenz-Mods auf GitHub](#28-referenz-mods-auf-github)
29. [Mod Sync Server API (Pre-Join)](#29-mod-sync-server-api-pre-join)

---

## 1. Quellen & Tools

| Ressource | URL / Pfad | Inhalt |
|---|---|---|
| **GDN LUADOC** (offiziell) | `gdn.giants-software.com/documentation_scripting_fs25.php` | Script-API (47 Kategorien) + Engine-API (33 Kategorien). Offen zugänglich. JavaScript-gerendert – direkte URLs: `?version=engine&category=X&function=Y` |
| **GDN Dokumentation** (offiziell) | `gdn.giants-software.com/documentation.php` | i3d-Format-Spec, Artwork Guide, Mapping Tutorials, DCC-Exporter-Guides (Blender/Maya), Shader-Doku |
| **GDN Video-Tutorials** (offiziell) | `gdn.giants-software.com/videoTutorials2.php` | Offizielle Schritt-für-Schritt-Videos: Map-Erstellung, Sound, Shader, Fahrzeuge |
| **GDN Scripting-Tutorials** (offiziell) | `gdn.giants-software.com/tutorials.php` | Schriftliche Scripting-Guides von GIANTS |
| **GDN Forum FS25** | `gdn.giants-software.com/forum_category.php?categoryId=25` | Community Q&A, FS25-spezifische Threads, XML-Referenz-Thread |
| **Community LuaDoc** | `umbraprior.github.io/FS25-Community-LUADOC/` | Vollständige Community-gepflegte Doku mit Suchfunktion; GitHub: `github.com/umbraprior/FS25-Community-LUADOC` |
| **FS25 Lua Scripting Repo** | `github.com/Dukefarming/FS25-lua-scripting` | Original-Quelldateien aus dem FS25 `dataS`-Ordner. Vehicle.lua (187 KB), VehicleMotor.lua (106 KB), VehicleDebug.lua (122 KB) |
| **FS25 Lua API Repo** | `github.com/MyGameSteamOfficial/fs25-lua-api` | Alle Specialization-Quelldateien (150+ Specs), Utils, Triggers, Placeables |
| **Scripting-Buch (Open Access)** | `library.oapen.org/handle/20.500.12657/86976` | 343 S. „Scripting FS with Lua" (Brumbaugh/Leithner, 2024) – kostenlos als PDF. Springer-Kauf: `link.springer.com/book/10.1007/979-8-8688-0060-3` |
| **Modding Tutorials DLC 6.0** (offiziell) | `farming-simulator.com/dlc-detail.php?dlc_id=fs25mt60` | Offizielles Tutorial-DLC: komplette Fahrzeug-Pipeline von Blender → GE → modDesc → ModHub |
| **Offizielles YouTube-Playlist** (offiziell) | `youtube.com/playlist?list=PL3sbfoaUaOqoMsbYgK9VmU6R3n3tCG7fa` | Video-Tutorials von GIANTS: Map-Erstellung, Shader, Fahrzeuge, Sounds |
| **AlfaMods FS25 Guides** | `alfamods.no/faq/fs25-modding/` | Praxisnahe Schritt-für-Schritt-Guides: Map-Mod, Vehicle-Mod, Skins, dataS-Ordner-Erklärung |
| **maps4fs** | `maps4fs.xyz` · `github.com/iwatkot/maps4fs` | Echtworld-Karten aus OpenStreetMap+SRTM generieren; Dokumentation erklärt FS25-Map-Dateistruktur detailliert |
| **METools für GE10** | `github.com/modelleicher/FS25-METools-for-Giants-Editor-10` | GE10-Skripte: Spline Exporter, Terrain-Höhe interpoliert setzen, Objekte entlang Splines platzieren |
| **FS25 ModTemplate** | `github.com/Jos-Modding/FS25_ModTemplate` | ModHub-konformes Mod-Template (descVersion 92+, korrekte Ordnerstruktur, Icon-Vorlage) |
| **Courseplay FS25** | `github.com/Courseplay/Courseplay_FS25` | Großes, gut strukturiertes Open-Source-Lua-Mod als Referenz-Architektur mit GitHub-Actions-CI |
| **FS25 UniversalAutoload** | `github.com/loki79uk/FS25_UniversalAutoload` | Ausgereifte, dokumentierte Spec-Implementierung; gutes Referenz-Beispiel für XML-Struktur |
| **ModLand Forum** | `modland.net/forums/farming-simulator-25/modding-discussion` | Community-Tutorials, FS22→FS25-Konvertierung, Dokumentations-Threads |
| **FarmerBoys Modding** | `farmerboysmodding.com` | Community-Forum mit dedizierten FS25-Fahrzeug-Tutorials, Blender-Tipps |
| **GIANTS Editor v10** | Download via GDN → Downloads | i3d-Modelle erstellen/exportieren, Terrain, Shader, GE-Skripte ausführen |
| **GIANTS Studio** | Download via GDN → Downloads | IDE + Remote-Debugger für Lua-Skripte |

### GDN API Kategorien (FS25 v1.15.0.0)

**Script API** (47 Kategorien): Activatables, AI, Animals, Animation, Base, Boatyard, Collections, Components, Configurations, Contracts, Data, Debug, Economy, Elements, Errors, Events, Extensions, Farms, Ferry, Field, FillTypes, Fruits, Graphical, Graphics, GuidedTour, GUI, Handtools, Hud, I3d, Input, Instances, Jobs, Materials, Misc, Missions, Networking, Objects, Parameters, Placeables, Placement, Player, Rollercoaster, Ship, Shop, Sounds, Specialization, Specializations, StateMachine, Tasks, Triggers, Utils, Vehicles, Weather, Wheels

**Engine API** (33 Kategorien): Animation, Camera, Debug, Entity, Fillplanes, Foliage, General, I3D, Input, Lighting, Math, NavMesh, Network, Node, NoteNode, Overlays, Particle System, Physics, PointList2D, Precipitation, Rendering, ShallowWaterSimulation, Shape, Sound, Spline, String, Terrain, Text Rendering, Tire Track, VoiceChat, XML

---

## 2. Mod-Ordnerstruktur

```
MeinMod/
├── modDesc.xml                       ← Pflicht-Einstiegspunkt
├── icon_MeinMod.dds                  ← 256×256, Mod-Icon im ModHub
├── scripts/
│   ├── specializations/              ← Fahrzeug-Specializations (.lua)
│   ├── placeables/                   ← Placeable-Specializations (.lua)
│   └── events/                       ← Netzwerk-Events (.lua)
├── vehicles/
│   └── meinFahrzeug/
│       ├── meinFahrzeug.xml
│       └── meinFahrzeug.i3d
├── placeables/
│   └── meinObjekt/
│       ├── meinObjekt.xml
│       ├── meinObjekt.i3d
│       └── store_meinObjekt.dds      ← Store-Vorschaubild
├── sounds/
│   └── meinSound.ogg
└── i18n/
    ├── de.xml
    └── en.xml
```

---

## 3. modDesc.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<modDesc descVersion="72">            <!-- 72 = FS25 -->

    <author>Dein Name</author>
    <version>1.0.0.0</version>        <!-- Major.Minor.Patch.Build -->

    <title>
        <de>Deutscher Titel</de>
        <en>English Title</en>
    </title>
    <description>
        <de>Beschreibung</de>
        <en>Description</en>
    </description>

    <iconFilename>icon_MeinMod.dds</iconFilename>
    <multiplayer supported="true"/>

    <!-- ── Vehicle Specializations ── -->
    <specializations>
        <specialization
            name="meinSpec"
            className="MeinSpec"
            filename="scripts/specializations/MeinSpec.lua"/>
    </specializations>

    <!-- ── Fahrzeugtypen erweitern (fremde Mods) ── -->
    <vehicleTypeExtensions>
        <vehicleTypeExtension typeName="tractor">
            <specialization name="MeinMod.meinSpec"/>
        </vehicleTypeExtension>
    </vehicleTypeExtensions>

    <!-- ── Placeable-Typen ── -->
    <placeableTypes>
        <placeableType name="meinPlaceable">
            <specialization
                name="placeable.meinPlaceable"
                className="MeinPlaceable"
                filename="scripts/placeables/MeinPlaceable.lua"/>
        </placeableType>
    </placeableTypes>

    <!-- ── Store-Einträge ── -->
    <storeItems>
        <storeItem xmlFilename="vehicles/meinFahrzeug/meinFahrzeug.xml"/>
        <storeItem xmlFilename="placeables/meinObjekt/meinObjekt.xml"/>
    </storeItems>

    <!-- ── Übersetzungen ── -->
    <l10n filenamePrefix="i18n/"/>

</modDesc>
```

### Placeable XML

```xml
<?xml version="1.0" encoding="utf-8"?>
<placeable type="MeinMod.meinPlaceable">
    <storeData>
        <name>$l10n_meinObjekt_name</name>
        <description>$l10n_meinObjekt_desc</description>
        <price>5000</price>
        <dailyUpkeep>10</dailyUpkeep>
        <category>UTILITY</category>     <!-- PRODUCTION | UTILITY | DECORATION | ANIMAL -->
        <isSellable>true</isSellable>
        <image>placeables/meinObjekt/store.dds</image>
        <iconFilename>placeables/meinObjekt/store.dds</iconFilename>
        <specs>
            <meinWert>42</meinWert>      <!-- Angezeigt im Shop -->
        </specs>
    </storeData>
    <filename>placeables/meinObjekt/meinObjekt.i3d</filename>
    <meinPlaceable>
        <wert>42</wert>
    </meinPlaceable>
</placeable>
```

---

## 4. Lua Klassen-Pattern

FS25 verwendet Metatabel-basiertes OOP. **Alle** Klassen folgen diesem Muster:

```lua
-- ── Klasse deklarieren ──────────────────────────────────────────────────────
MeineKlasse = {}
local MeineKlasse_mt = Class(MeineKlasse)             -- keine Elternklasse
local MeineKlasse_mt = Class(MeineKlasse, ElternKlasse) -- mit Vererbung

-- ── Konstruktor ─────────────────────────────────────────────────────────────
function MeineKlasse.new(arg1, arg2)
    local self = setmetatable({}, MeineKlasse_mt)
    self.wert = arg1
    return self
end

-- ── Instanz-Methode (self = Instanz) ────────────────────────────────────────
function MeineKlasse:tuWas(param)
    return self.wert + param
end

-- ── Statische Funktion (kein self) ──────────────────────────────────────────
function MeineKlasse.hilfsMethode(a, b)
    return a + b
end

-- ── Eltern-Methode aufrufen (super) ─────────────────────────────────────────
function MeineKlasse:tuWas(param)
    MeineKlasse_mt.__index.tuWas(self, param)  -- super()
    -- eigene Logik danach
end
```

---

## 5. Vehicle Specialization

### Vollständige Registrierung

```lua
MeinSpec = {}
local MeinSpec_mt = Class(MeinSpec)

-- 1. Voraussetzungen prüfen (andere Specs müssen vorhanden sein)
function MeinSpec.prerequisitesPresent(specializations)
    return SpecializationUtil.hasSpecialization(Motorized, specializations)
    -- weitere: Enterable, Drivable, Attachable, FillUnit, WorkArea …
end

-- 2. XML-Schema + Savegame-Schema registrieren
function MeinSpec.initSpecialization()
    local schema = Vehicle.xmlSchema
    schema:setXMLSpecializationType("MeinSpec")   -- Gruppenname für Debugging

    schema:register(XMLValueType.FLOAT,  "vehicle.meinSpec#floatWert",  "Beschreibung", 100.0)
    schema:register(XMLValueType.INT,    "vehicle.meinSpec#intWert",    "Beschreibung", 5)
    schema:register(XMLValueType.BOOL,   "vehicle.meinSpec#boolWert",   "Beschreibung", false)
    schema:register(XMLValueType.STRING, "vehicle.meinSpec#stringWert", "Beschreibung", "default")
    schema:register(XMLValueType.NODE_INDEX, "vehicle.meinSpec#node",   "Node-Referenz")
    schema:register(XMLValueType.VECTOR_3,   "vehicle.meinSpec#vec",    "Vektor", "0 0 0")
    schema:register(XMLValueType.L10N_STRING,"vehicle.meinSpec#text",   "Übersetzungsschlüssel")

    schema:setXMLSpecializationType()  -- Ende der Gruppe

    -- Savegame-Pfad (separates Schema!)
    local sg = Vehicle.xmlSchemaSavegame
    sg:register(XMLValueType.FLOAT, "vehicles.vehicle(?).meinSpec#floatWert", "Gespeicherter Wert")
end

-- 3. Neue Funktionen am vehicleType registrieren
function MeinSpec.registerFunctions(vehicleType)
    SpecializationUtil.registerFunction(vehicleType, "meineFunktion",   MeinSpec.meineFunktion)
    SpecializationUtil.registerFunction(vehicleType, "getWert",         MeinSpec.getWert)
    SpecializationUtil.registerFunction(vehicleType, "setWert",         MeinSpec.setWert)
end

-- 4. Bestehende Funktionen überschreiben (superFunc-Pattern)
function MeinSpec.registerOverwrittenFunctions(vehicleType)
    SpecializationUtil.registerOverwrittenFunction(vehicleType, "getCanMotorRun", MeinSpec.getCanMotorRun)
end

-- 5. Custom Events registrieren
function MeinSpec.registerEvents(vehicleType)
    SpecializationUtil.registerEvent(vehicleType, "onMeinEvent")
end

-- 6. Event-Listener registrieren
function MeinSpec.registerEventListeners(vehicleType)
    SpecializationUtil.registerEventListener(vehicleType, "onLoad",               MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onPostLoad",           MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onSaveToXMLFile",      MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onUpdate",             MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onUpdateTick",         MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onDraw",               MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onDelete",             MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onReadStream",         MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onWriteStream",        MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onReadUpdateStream",   MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onWriteUpdateStream",  MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onRegisterActionEvents", MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onEnterVehicle",       MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onLeaveVehicle",       MeinSpec)
end
```

### Lifecycle-Methoden

```lua
-- Laden (beim Spielstart / Speicherstand laden)
function MeinSpec:onLoad(savegame)
    local spec = {}
    self.spec_meinSpec = spec     -- IMMER so benennen: spec_[klassenname_lowercase]

    -- Aus Fahrzeug-XML lesen
    spec.floatWert = self.xmlFile:getValue("vehicle.meinSpec#floatWert", 100.0)
    spec.node      = self.xmlFile:getValue("vehicle.meinSpec#node",      nil, self.components, self.i3dMappings)

    -- DirtyFlag für Netzwerk-Sync anlegen
    spec.dirtyFlag = self:getNextDirtyFlag()

    -- Savegame laden
    if savegame ~= nil then
        spec.floatWert = savegame.xmlFile:getValue(
            savegame.key .. ".meinSpec#floatWert", spec.floatWert)
    end
end

-- Nach allen Specializations geladen
function MeinSpec:onPostLoad(savegame)
    -- Hier auf andere Specs zugreifen, die evtl. noch nicht in onLoad verfügbar waren
end

-- Speichern
function MeinSpec:onSaveToXMLFile(xmlFile, key, usedModNames)
    local spec = self.spec_meinSpec
    xmlFile:setValue(key .. ".meinSpec#floatWert", spec.floatWert)
end

-- Aufräumen (Fahrzeug wird entfernt)
function MeinSpec:onDelete()
    -- Timer, Trigger, globale Referenzen aufräumen
end

-- Update (jeden Frame, server UND client)
function MeinSpec:onUpdate(dt, isActiveForInput, isActiveForInputIgnoreSelection, isSynchronized)
    -- dt = Millisekunden seit letztem Frame
    if self.isServer then
        -- Nur Server-Logik hier
        self:raiseDirtyFlags(self.spec_meinSpec.dirtyFlag)
    end
end

-- UpdateTick (alle N ms, weniger frequent als onUpdate)
function MeinSpec:onUpdateTick(dt, isActiveForInput, isActiveForInputIgnoreSelection, isSynchronized)
end

-- HUD zeichnen (nur Client, nur wenn Spieler im Fahrzeug sitzt)
function MeinSpec:onDraw()
    if not self.isActiveForInputIgnoreSelection then return end
    -- Hier rendern
end

-- Spieler betritt Fahrzeug
function MeinSpec:onEnterVehicle(isControlling)
end

-- Spieler verlässt Fahrzeug
function MeinSpec:onLeaveVehicle()
end
```

### Overwritten Function Pattern (superFunc)

```lua
function MeinSpec:getCanMotorRun(superFunc)
    -- Erst die originale Funktion aufrufen
    if not superFunc(self) then return false end
    -- Eigene Bedingung hinzufügen
    return self.spec_meinSpec.batteryLevel > 0
end
```

### Spec an Fahrzeug-Typ hinzufügen

**In eigenem Fahrzeug-XML:**
```xml
<vehicle type="basegame.tractor">
    <specializations>
        <specialization name="MeinMod.meinSpec"/>
    </specializations>
</vehicle>
```

**Fremde Typen erweitern (modDesc.xml):**
```xml
<vehicleTypeExtensions>
    <vehicleTypeExtension typeName="basegame.tractor">
        <specialization name="MeinMod.meinSpec"/>
    </vehicleTypeExtension>
</vehicleTypeExtensions>
```

---

## 6. Placeable Specialization

```lua
MeinPlaceable = {}
local MeinPlaceable_mt = Class(MeinPlaceable)

function MeinPlaceable.initSpecialization()
    local schema = Placeable.xmlSchema     -- ← Placeable, nicht Vehicle!
    schema:register(XMLValueType.FLOAT, "placeable.meinPlaceable#wert", "Beschreibung", 10)
end

function MeinPlaceable.registerFunctions(placeableType)
    SpecializationUtil.registerFunction(placeableType, "getWert", MeinPlaceable.getWert)
end

function MeinPlaceable.registerEventListeners(placeableType)
    SpecializationUtil.registerEventListener(placeableType, "onLoad",              MeinPlaceable)
    SpecializationUtil.registerEventListener(placeableType, "onDelete",            MeinPlaceable)
    SpecializationUtil.registerEventListener(placeableType, "onFinalizePlacement", MeinPlaceable)  -- rootNode ist jetzt gültig!
    SpecializationUtil.registerEventListener(placeableType, "onUpdate",            MeinPlaceable)
    SpecializationUtil.registerEventListener(placeableType, "onReadStream",        MeinPlaceable)
    SpecializationUtil.registerEventListener(placeableType, "onWriteStream",       MeinPlaceable)
end

function MeinPlaceable:onLoad(savegame)
    local spec = {}
    self.spec_meinPlaceable = spec
    spec.wert = self.xmlFile:getValue("placeable.meinPlaceable#wert", 10)
end

-- WICHTIG: rootNode ist erst in onFinalizePlacement verfügbar
function MeinPlaceable:onFinalizePlacement()
    local x, y, z = getWorldTranslation(self.rootNode)
    -- z.B. in globalem Manager registrieren
end

function MeinPlaceable:onDelete()
    -- Aus globalem Manager austragen
end

function MeinPlaceable:onUpdate(dt)
    if not self.isServer then return end
end
```

---

## 7. Wichtige Event-Listener

Alle verfügbaren Events (aus Vehicle.lua / FS25 Quellcode):

| Event | Wann | Parameter |
|---|---|---|
| `onLoad` | Laden aus XML | `savegame` |
| `onPostLoad` | Nach allen Specs geladen | `savegame` |
| `onDelete` | Fahrzeug/Placeable wird entfernt | – |
| `onUpdate` | Jeden Frame | `dt, isActiveForInput, isActiveForInputIgnoreSelection, isSynchronized` |
| `onUpdateTick` | Jeder Tick (reduziert) | `dt, …` |
| `onDraw` | Render-Phase | – |
| `onSaveToXMLFile` | Speichern | `xmlFile, key, usedModNames` |
| `onReadStream` | Netzwerk-Join (vollständiger State) | `streamId, connection` |
| `onWriteStream` | Netzwerk-Join | `streamId, connection` |
| `onReadUpdateStream` | Netzwerk-Delta-Update | `streamId, timestamp, connection` |
| `onWriteUpdateStream` | Netzwerk-Delta-Update | `streamId, connection, dirtyMask` |
| `onRegisterActionEvents` | Input-Events registrieren | `isActiveForInput, isActiveForInputIgnoreSelection` |
| `onEnterVehicle` | Spieler betritt Fahrzeug | `isControlling` |
| `onLeaveVehicle` | Spieler verlässt Fahrzeug | – |
| `onSetBroken` | Fahrzeug kaputt | – |
| `onFinalizePlacement` | Placeable: nach Platzieren | – (nur Placeables) |
| `onVehiclePhysicsUpdate` | Physik-Update (Drivable-Event) | – |

---

## 8. Netzwerk & Multiplayer

### Vollständiger State beim Verbinden (Join)

```lua
function MeinSpec:onWriteStream(streamId, connection)
    local spec = self.spec_meinSpec
    streamWriteFloat32(streamId,  spec.floatWert)
    streamWriteUInt8(streamId,    spec.intWert)        -- 0-255
    streamWriteInt8(streamId,     spec.signedInt)      -- -128 bis 127
    streamWriteInt32(streamId,    spec.bigInt)
    streamWriteBool(streamId,     spec.boolWert)
    streamWriteString(streamId,   spec.text)
end

function MeinSpec:onReadStream(streamId, connection)
    local spec = self.spec_meinSpec
    spec.floatWert  = streamReadFloat32(streamId)
    spec.intWert    = streamReadUInt8(streamId)
    spec.signedInt  = streamReadInt8(streamId)
    spec.bigInt     = streamReadInt32(streamId)
    spec.boolWert   = streamReadBool(streamId)
    spec.text       = streamReadString(streamId)
end
```

### Delta-Updates (nur geänderte Daten)

```lua
-- In onLoad: DirtyFlag anlegen
spec.dirtyFlag = self:getNextDirtyFlag()

-- Wenn sich etwas ändert (server-seitig):
self:raiseDirtyFlags(spec.dirtyFlag)

-- Delta schreiben (bool als Header: hat sich geändert?)
function MeinSpec:onWriteUpdateStream(streamId, connection, dirtyMask)
    if streamWriteBool(streamId, bitAND(dirtyMask, self.spec_meinSpec.dirtyFlag) ~= 0) then
        streamWriteFloat32(streamId, self.spec_meinSpec.floatWert)
        streamWriteBool(streamId,    self.spec_meinSpec.boolWert)
    end
end

function MeinSpec:onReadUpdateStream(streamId, timestamp, connection)
    if streamReadBool(streamId) then
        self.spec_meinSpec.floatWert = streamReadFloat32(streamId)
        self.spec_meinSpec.boolWert  = streamReadBool(streamId)
    end
end
```

### Stream-Funktionen Übersicht

| Funktion | Bits | Wertebereich |
|---|---|---|
| `streamWriteBool / streamReadBool` | 1 | true/false |
| `streamWriteUInt8 / streamReadUInt8` | 8 | 0–255 |
| `streamWriteInt8 / streamReadInt8` | 8 | -128–127 |
| `streamWriteUInt16 / streamReadUInt16` | 16 | 0–65535 |
| `streamWriteInt16 / streamReadInt16` | 16 | -32768–32767 |
| `streamWriteInt32 / streamReadInt32` | 32 | -2^31–2^31 |
| `streamWriteFloat32 / streamReadFloat32` | 32 | float |
| `streamWriteString / streamReadString` | variabel | String |

---

## 9. XML Schema registrieren

### XMLValueType Typen

```lua
XMLValueType.BOOL          -- true / false
XMLValueType.INT           -- Ganzzahl
XMLValueType.FLOAT         -- Fließkommazahl
XMLValueType.STRING        -- Text
XMLValueType.L10N_STRING   -- Übersetzungsschlüssel ($l10n_...)
XMLValueType.NODE_INDEX    -- i3d-Node-Referenz (z.B. "0>1>2")
XMLValueType.VECTOR_2      -- "x y"
XMLValueType.VECTOR_3      -- "x y z"
XMLValueType.VECTOR_4      -- "x y z w"
XMLValueType.ANGLE         -- Grad → wird zu Radiant konvertiert
XMLValueType.TIME          -- Zeit in Sekunden
XMLValueType.COLOR         -- "r g b a"
```

### Registrierung

```lua
local schema = Vehicle.xmlSchema        -- für Fahrzeuge
local schema = Placeable.xmlSchema      -- für Placeables

-- Einfacher Wert
schema:register(XMLValueType.FLOAT, "vehicle.spec#attribut", "Beschreibung", defaultWert)

-- Arrayelement (?)
schema:register(XMLValueType.FLOAT, "vehicle.spec.element(?)#wert", "Wert", 0)

-- Nested
schema:register(XMLValueType.NODE_INDEX, "vehicle.spec.nodes.node(?)#node", "Node")

-- Werte lesen
local wert = self.xmlFile:getValue("vehicle.spec#attribut", defaultWert)
local node = self.xmlFile:getValue("vehicle.spec#node", nil, self.components, self.i3dMappings)

-- Array lesen
local i = 0
while true do
    local key = string.format("vehicle.spec.element(%d)", i)
    if not self.xmlFile:hasProperty(key) then break end
    local wert = self.xmlFile:getValue(key .. "#wert", 0)
    i = i + 1
end

-- Savegame lesen/schreiben
local sg = Vehicle.xmlSchemaSavegame
sg:register(XMLValueType.FLOAT, "vehicles.vehicle(?).spec#wert", "Gespeicherter Wert")

-- In onLoad:
if savegame ~= nil then
    wert = savegame.xmlFile:getValue(savegame.key .. ".spec#wert", wert)
end
-- In onSaveToXMLFile:
xmlFile:setValue(key .. ".spec#wert", wert)
```

---

## 10. Engine API – Node

Alle Node-Funktionen arbeiten mit `entityId` (dem Node-Handle aus dem i3d):

### Position & Rotation

```lua
-- Weltkoordinaten lesen
local x, y, z = getWorldTranslation(nodeId)

-- Weltkoordinaten setzen
setWorldTranslation(nodeId, x, y, z)

-- Lokale Position lesen (relativ zum Eltern-Node)
local x, y, z = getTranslation(nodeId)

-- Lokale Position setzen
setTranslation(nodeId, x, y, z)

-- Rotation (Euler, Radiant)
local rx, ry, rz = getRotation(nodeId)
setRotation(nodeId, rx, ry, rz)

-- Weltrotation
local rx, ry, rz = getWorldRotation(nodeId)
setWorldRotation(nodeId, rx, ry, rz)

-- Scale
local sx, sy, sz = getScale(nodeId)
setScale(nodeId, sx, sy, sz)

-- Sichtbarkeit
local visible = getVisibility(nodeId)   -- bool
setVisibility(nodeId, true)
```

### Richtungsvektoren

```lua
-- Weltrichtung eines Nodes (vorwärts, hoch, rechts)
local dx, dy, dz = localDirectionToWorld(nodeId, 0, 0, 1)  -- forward

-- Lokale in Weltkoordinaten
local wx, wy, wz = localToWorld(nodeId, lx, ly, lz)

-- Welt in lokale Koordinaten
local lx, ly, lz = worldToLocal(nodeId, wx, wy, wz)

-- Weltrichtung → lokal
local lx, ly, lz = worldDirectionToLocal(nodeId, wx, wy, wz)
```

### Abstände

```lua
-- Abstand zwischen zwei Nodes
local dist = calcDistanceFrom(nodeA, nodeB)

-- Abstand² (schneller, kein sqrt)
local distSq = calcDistanceSquaredFrom(nodeA, nodeB)

-- Manuell (mit MathUtil)
local x1,y1,z1 = getWorldTranslation(nodeA)
local x2,y2,z2 = getWorldTranslation(nodeB)
local dist = MathUtil.vector3Length(x1-x2, y1-y2, z1-z2)
```

### Hierarchie

```lua
-- Kinder
local count   = getNumOfChildren(nodeId)
local child   = getChildAt(nodeId, index)      -- 0-basiert
local child   = getChild(nodeId, "name")       -- nach Name

-- Eltern
local parent  = getParent(nodeId)

-- Root-Node
local root    = getRootNode()

-- Verknüpfen / Trennen
link(parentNode, childNode)
unlink(nodeId)

-- Node erstellen
local newNode = createTransformGroup("name")
link(parentNode, newNode)

-- Klonen
local clone = clone(nodeId, true, false, true)  -- (node, dynPhys, locked, withChildren)
```

### User-Attribute (i3d Custom Properties)

```lua
local wert = getUserAttribute(nodeId, "attributName")
setUserAttribute(nodeId, "attributName", "string", wert)
-- Typen: "string", "integer", "float", "boolean", "scriptCallback"
```

---

## 11. Engine API – Math & Bit

### Bit-Operationen

```lua
bitAND(a, b)          -- bitweises UND
bitOR(a, b)           -- bitweises ODER
bitXOR(a, b)          -- bitweises XOR
bitNOT(a)             -- bitweises NICHT
bitShiftLeft(a, n)    -- Links-Shift um n Bits
bitShiftRight(a, n)   -- Rechts-Shift um n Bits
bitHighestSet(a)       -- Index des höchsten gesetzten Bits
```

### Wichtig: DirtyFlag-Check

```lua
-- Standard-Pattern für Dirty-Flags
if bitAND(dirtyMask, spec.dirtyFlag) ~= 0 then
    -- Daten haben sich verändert
end
```

### MathUtil (Script-API)

```lua
-- Vektoren
MathUtil.vector3Length(x, y, z)              -- Betrag eines 3D-Vektors
MathUtil.vector3Normalize(x, y, z)           -- normalisiert → gibt x,y,z zurück
MathUtil.vector2Length(x, y)
MathUtil.dotProduct(x1,y1,z1, x2,y2,z2)

-- Interpolation
MathUtil.lerp(a, b, t)                       -- lineare Interpolation, t ∈ [0,1]
MathUtil.clamp(wert, min, max)               -- begrenzt auf [min, max]

-- Winkel
MathUtil.getAngleDifference(a, b)            -- Differenz zweier Winkel (Radiant)
MathUtil.normalizeAngle(angle)               -- Normalisiert auf [0, 2π]

-- Richtung
MathUtil.eulerToDirection(yaw, pitch)
    -- → float x, float y, float z  (Richtungsvektor)

MathUtil.directionToPitchYaw(dx, dy, dz)
    -- → float pitch, float yaw

-- Zufallszahlen
math.random()                                -- 0.0 – 1.0
math.random(min, max)                        -- Ganzzahl min–max
```

### Lua Standard-Mathematik

```lua
math.sin(rad)   math.cos(rad)   math.tan(rad)
math.asin(x)    math.acos(x)    math.atan2(y, x)
math.abs(x)     math.floor(x)   math.ceil(x)
math.sqrt(x)    math.max(a,b)   math.min(a,b)
math.huge                        -- positiv unendlich
math.pi                          -- π

-- Zeit-Umrechnung (dt ist immer in ms)
local sek    = dt / 1000
local min    = dt / 60000
local stunden = dt / 3600000
```

---

## 12. Engine API – HUD & Rendering

```lua
-- ── Overlays (Rechtecke) ────────────────────────────────────────────────────
-- renderOverlay(overlayId, x, y, width, height, r, g, b, a)
-- overlayId = nil → einfarbiges Rechteck
renderOverlay(nil, 0.02, 0.15, 0.10, 0.02, 0, 0, 0, 0.65)   -- schwarzer Hintergrund
renderOverlay(nil, 0.02, 0.15, 0.07, 0.02, 0.1, 0.8, 0.2, 1) -- grüner Balken

-- Eigenes Overlay erstellen (aus DDS-Datei)
local overlayId = createImageOverlay("icons/myIcon.dds")
setOverlayColor(overlayId, r, g, b, a)
renderOverlay(overlayId, x, y, w, h)
deleteImage(overlayId)   -- in onDelete aufrufen!

-- ── Text ────────────────────────────────────────────────────────────────────
setTextAlignment(RenderText.ALIGN_LEFT)     -- oder ALIGN_CENTER, ALIGN_RIGHT
setTextColor(r, g, b, a)                    -- r,g,b,a ∈ [0,1]
setTextBold(true)
setTextVerticalAlignment(RenderText.VERTICAL_ALIGN_MIDDLE)
renderText(x, y, size, "text")
-- x, y = Bildschirmposition (0-1, links unten = 0,0)
-- size = Schriftgröße in Bildschirmanteil (z.B. 0.015)

-- String formatieren
string.format("%.1f kW", 22.5)     -- "22.5 kW"
string.format("%d/%d", 3, 4)       -- "3/4"
```

---

## 13. Engine API – Sound

```lua
-- Sound aus SoundManager laden und abspielen
local sampleFile = "sounds/meinSound.ogg"
local sample = g_soundManager:loadSample(
    self, {}, sampleFile, self.baseDirectory, nil, 1, 1, 0, nil, false, nil)

g_soundManager:playSample(sample)
g_soundManager:stopSample(sample)
g_soundManager:setVolume(sample, 0.8)                -- 0-1
g_soundManager:setSamplePitch(sample, 1.2)           -- 1.0 = normal
g_soundManager:deleteSample(sample)                  -- in onDelete!

-- Prüfen ob Sound läuft
local playing = g_soundManager:getIsSamplePlaying(sample)

-- Loop-Sound Parameter (für Motor-Sounds)
g_soundManager:setSamplesLoopSynthesisParameters(samples, rpmPercentage, loadPercentage)
```

---

## 14. Engine API – Input

```lua
-- Aktion registrieren (in onRegisterActionEvents)
function MeinSpec:onRegisterActionEvents(isActiveForInput, isActiveForInputIgnoreSelection)
    if self.isClient then
        local spec = self.spec_meinSpec
        self:clearActionEventsTable(spec.actionEvents)

        local _, eventId = self:addActionEvent(
            spec.actionEvents,
            InputAction.MEINE_AKTION,     -- Aktion aus input bindings
            self,
            MeinSpec.actionEventMeineAktion,
            false,                         -- triggerUp
            true,                          -- triggerDown
            false,                         -- triggerAlways
            true,                          -- startActive
            nil
        )
        g_inputBinding:setActionEventText(eventId, g_i18n:getText("action_meineAktion"))
        g_inputBinding:setActionEventTextVisibility(eventId, true)
    end
end

-- Callback
function MeinSpec.actionEventMeineAktion(self, actionName, inputValue, callbackState, isAnalog)
    if self.isServer then
        -- Direkt ausführen
    else
        -- Event an Server schicken
        MeinEventClass.sendEvent(self)
    end
end

-- Gamepad-Info
getGamepadAxisLabel(axisNumber, gamepadIndex)    -- → string Label
getGamepadButtonLabel(buttonNumber, gamepadIndex)
getNumOfGamepads()                               -- → int
```

---

## 15. Engine API – Allgemein (print, XML)

```lua
-- ── Logging ─────────────────────────────────────────────────────────────────
print("Normaler Text")                        -- weiß
printWarning("Warnung")                       -- gelb, öffnet Konsole
printError("Fehler")                          -- orange, öffnet Konsole
printCallstack()                              -- zeigt Aufruf-Stack

-- FS25 Logging-Klasse (besser als print)
Logging.info("Nachricht: %s", variable)
Logging.warning("Warnung: %d", zahl)
Logging.error("Fehler in %s", funcName)
Logging.devInfo("Nur im Dev-Mode sichtbar")

-- ── XML Engine-Funktionen ────────────────────────────────────────────────────
local xmlFile = loadXMLFile("name", "pfad/zur/datei.xml")
local wert    = getXMLFloat(xmlFile, "key#attribut")
local wert    = getXMLInt(xmlFile, "key#attribut")
local wert    = getXMLBool(xmlFile, "key#attribut")
local wert    = getXMLString(xmlFile, "key#attribut")
setXMLFloat(xmlFile, "key#attribut", wert)
saveXMLFile(xmlFile)
delete(xmlFile)                               -- MUSS aufgerufen werden!

-- Besser: XMLFile-Wrapper (FS25 Standard)
local xmlFile = XMLFile.load("name", "pfad.xml", schema)
xmlFile:getValue("vehicle.spec#attribut", default)
xmlFile:setValue("vehicle.spec#attribut", wert)
xmlFile:hasProperty("vehicle.spec")
xmlFile:iterate("vehicle.spec.items", function(i, key) ... end)
xmlFile:delete()

-- ── String-Funktionen ────────────────────────────────────────────────────────
string.format("%s: %.2f", name, wert)
string.len(s)
string.upper(s)   string.lower(s)
string.sub(s, start, ende)
string.find(s, pattern)
string.isNilOrWhitespace(s)   -- FS25-eigene Erweiterung
tostring(wert)
tonumber(str)

-- UTF-8 Funktionen (Engine)
utf8Strlen(str)
utf8Substr(str, start, len)
utf8ToUpper(str)   utf8ToLower(str)

-- ── Tabellen-Utilities ───────────────────────────────────────────────────────
table.insert(t, wert)
table.remove(t, index)
table.clone(t, depth)          -- FS25-eigene Funktion: tiefe Kopie
table.removeElement(t, elem)   -- FS25-eigene: entfernt Element, gibt bool zurück

-- ── Globale Objekte ──────────────────────────────────────────────────────────
g_currentMission           -- Laufende Mission
g_currentMission.vehicles  -- Tabelle aller Fahrzeuge
g_currentMission.time      -- Spielzeit in ms
g_i18n                     -- Übersetzungs-Manager
g_i18n:getText("key")      -- Text übersetzen
g_soundManager             -- Sound-Manager
g_inputBinding             -- Input-Binding-Manager
```

---

## 16. Script API – SpecializationUtil

Alle Funktionen aus dem Quellcode (`SpecializationUtil.lua`):

```lua
-- Spec in Liste prüfen
SpecializationUtil.hasSpecialization(SpecClass, specializations)
    -- @param SpecClass  die Klasse (z.B. Motorized)
    -- @param specializations  Liste aus prerequisitesPresent-Parameter
    -- @return boolean

-- Funktion registrieren
SpecializationUtil.registerFunction(objectType, funcName, func)

-- Bestehende Funktion überschreiben (fügt superFunc-Parameter hinzu)
SpecializationUtil.registerOverwrittenFunction(objectType, funcName, func)

-- Event registrieren (erstellt Listener-Liste)
SpecializationUtil.registerEvent(objectType, eventName)

-- Event-Listener registrieren
SpecializationUtil.registerEventListener(objectType, eventName, specClass)

-- Event-Listener entfernen
SpecializationUtil.removeEventListener(object, eventName, specClass)

-- Event auslösen (synchron)
SpecializationUtil.raiseEvent(object, eventName, ...)

-- Event auslösen (asynchron, über Task-Queue)
SpecializationUtil.raiseAsyncEvent(object, eventName, ...)

-- Typ-Funktionen in Zielobjekt kopieren
SpecializationUtil.copyTypeFunctionsInto(typeDef, target)

-- Spec-Daten in Typ-Klasse initialisieren
SpecializationUtil.initSpecializationsIntoTypeClass(typeManager, typeDef, target)

-- Loading Tasks (für async i3d-Loading)
local task = SpecializationUtil.createLoadingTask(typeClass, target)
SpecializationUtil.finishLoadingTask(typeClass, task)
SpecializationUtil.setLoadingStep(typeClass, loadingStep)
    -- loadingStep: SpecializationLoadStep.SYNCHRONIZED | LOADING | …
```

---

## 17. Script API – MathUtil

```lua
-- Vektor-Operationen
MathUtil.vector3Length(x, y, z)              -- float: Betrag
MathUtil.vector2Length(x, y)
MathUtil.vector3Normalize(x, y, z)           -- float x, y, z
MathUtil.dotProduct(x1,y1,z1, x2,y2,z2)     -- float: Skalarprodukt

-- Interpolation & Begrenzung
MathUtil.lerp(a, b, t)                       -- lineare Interpolation
MathUtil.clamp(wert, min, max)               -- Bereichsbegrenzung
MathUtil.inverseLerp(a, b, t)

-- Winkel
MathUtil.getAngleDifference(a, b)
MathUtil.normalizeAngle(angle)               -- normalisiert auf [0, 2π]
MathUtil.degToRad(deg)
MathUtil.radToDeg(rad)

-- Richtung
MathUtil.eulerToDirection(yaw, pitch)
    -- Berechnet Richtungsvektor aus Euler-Winkeln
    -- → float x, float y, float z

MathUtil.directionToPitchYaw(dx, dy, dz)
    -- → float pitch, float yaw
```

---

## 18. Script API – Vehicle-Klasse

Wichtige Felder und Methoden an jedem Fahrzeug-Objekt:

```lua
-- ── Felder (read-only) ───────────────────────────────────────────────────────
vehicle.isServer               -- bool: läuft auf Server?
vehicle.isClient               -- bool: läuft auf Client?
vehicle.isActiveForInput       -- bool: Spieler sitzt drin UND hat Kontrolle
vehicle.isActiveForInputIgnoreSelection  -- bool: Spieler sitzt drin
vehicle.rootNode               -- entityId: Haupt-Node
vehicle.configFileName         -- string: Pfad zur XML
vehicle.baseDirectory          -- string: Mod-Ordner

-- ── Allgemeine Methoden ──────────────────────────────────────────────────────
vehicle:getLastSpeed()         -- float: Geschwindigkeit in km/h
vehicle:getLastSpeed(true)     -- float: Geschwindigkeit des Anbauters (falls angebaut)
vehicle:getTotalMass()         -- float: Gesamtmasse in t

-- Schaden
vehicle:getVehicleDamage()     -- float: 0.0 = heil, 1.0 = kaputt

-- Netzwerk
vehicle:raiseDirtyFlags(dirtyFlag)      -- Markiert Daten als geändert
vehicle:getNextDirtyFlag()             -- Holt nächsten freien DirtyFlag-Slot
vehicle:addAsyncTask(func, name, bool) -- Aufgabe in Task-Queue einreihen

-- Alle Fahrzeuge in der Mission
for _, v in pairs(g_currentMission.vehicles) do
    if v.isElectricVehicle ~= nil then  -- eigene Spec prüfen
        print(v:getLastSpeed())
    end
end
```

---

## 19. Script API – Motorized Spec

```lua
-- ── Zustand ─────────────────────────────────────────────────────────────────
local spec = vehicle.spec_motorized

spec.motorStarted              -- bool (veraltet, besser getIsMotorStarted())
vehicle:getIsMotorStarted()    -- bool: Motor läuft
vehicle:getIsMotorInNeutral()  -- bool: Motor im Leerlauf
vehicle:getCanMotorRun()       -- bool: Darf Motor starten (Schaden, Kraftstoff …)

-- ── Motor-State ──────────────────────────────────────────────────────────────
vehicle:getMotorState()        -- MotorState.OFF | STARTING | ON
-- MotorState.OFF      = Motor aus
-- MotorState.STARTING = Motor startet
-- MotorState.ON       = Motor läuft

-- ── Werte ───────────────────────────────────────────────────────────────────
vehicle:getMotorLoadPercentage()   -- float: 0.0–1.0 (aktueller Last-% geschmeidigt)
vehicle:getMotorRpmPercentage()    -- float: 0.0–1.0
vehicle:getMotorRpmReal()          -- float: Echte RPM-Zahl

-- ── Kraftstoff ──────────────────────────────────────────────────────────────
-- Kraftstoff ist über FillUnit geregelt
-- spec_motorized.consumers[i].fillUnitIndex → FillUnit-Index
-- vehicle:getFillUnitFillLevel(fillUnitIndex)

-- ── Steuern ─────────────────────────────────────────────────────────────────
vehicle:startMotor(noEventSend)    -- Motor starten  (noEventSend = true: kein Netzwerk-Event)
vehicle:stopMotor(noEventSend)     -- Motor stoppen

-- ── Motor-Typ ───────────────────────────────────────────────────────────────
vehicle:getMotorType()             -- z.B. "diesel", "electric", "methane"
```

---

## 20. Script API – Drivable Spec

```lua
local spec = vehicle.spec_drivable

-- ── Cruise Control ───────────────────────────────────────────────────────────
vehicle:getCruiseControlState()    -- Drivable.CRUISECONTROL_STATE_OFF | FORWARD | BACKWARD
vehicle:getCruiseControlSpeed()    -- float: km/h
vehicle:setCruiseControlState(state, noEventSend)
vehicle:setCruiseControlMaxSpeed(speed)

-- ── Fahrrichtung ─────────────────────────────────────────────────────────────
vehicle:getIsDrivingForward()      -- bool
vehicle:getIsDrivingBackward()     -- bool
vehicle:getDrivingDirection()      -- int: 1 = vorwärts, -1 = rückwärts

-- ── Steuerung ────────────────────────────────────────────────────────────────
vehicle:getAxisForward()           -- float: Gaspedal -1 bis 1
vehicle:getAccelerationAxis()      -- float: Beschleunigungsachse
vehicle:getDecelerationAxis()      -- float: Bremsachse
vehicle:brakeToStop()              -- Fahrzeug bremsen bis Stillstand

-- ── Feststellbremse / Parken ─────────────────────────────────────────────────
-- HINWEIS: FS25 hat keine separate "parkingBrakeIsSet"-Eigenschaft in Drivable!
-- Prüfung ob Fahrzeug steht (zuverlässigste Methode):
local isParked = math.abs(vehicle:getLastSpeed()) < 0.5    -- km/h

-- Spielerkontrolle
vehicle:getIsVehicleControlledByPlayer()     -- bool
vehicle:getIsPlayerVehicleControlAllowed()   -- bool
```

---

## 21. Trigger-System

Trigger erkennen wenn Objekte eine Zone betreten/verlassen.

### In GIANTS Editor einrichten

1. Leere `TransformGroup` erstellen (Kind des Root-Nodes)
2. Name: z.B. `triggerNode`
3. Attributes → **Collision** aktivieren, **Trigger** = ✓
4. Collision Mask: `255` (erkennt alles) oder spezifisch
5. Scale auf gewünschten Bereich setzen (z.B. `2.5 × 1.0 × 2.5` für 5m×2m×5m-Rechteck)

### In Lua verwenden

```lua
-- Trigger registrieren
local triggerNode = self.xmlFile:getValue("placeable.meinSpec#triggerNode", nil, ...)
if triggerNode ~= nil then
    addTrigger(triggerNode, "onTriggerCallback", self)
end

-- Trigger-Callback
-- onEnter = true wenn Objekt die Zone betritt
-- onLeave = true wenn Objekt die Zone verlässt
-- onStay  = true wenn Objekt in der Zone bleibt (jeden Frame)
function MeinSpec:onTriggerCallback(triggerId, otherId, onEnter, onLeave, onStay)
    -- Fahrzeug-Objekt aus Node-ID holen
    local vehicle = g_currentMission.nodeToObject[otherId]
    if vehicle == nil or vehicle.spec_meinSpec == nil then return end

    if onEnter then
        -- Fahrzeug betritt Trigger
    elseif onLeave then
        -- Fahrzeug verlässt Trigger
    end
end

-- Trigger entfernen (in onDelete!)
if triggerNode ~= nil then
    removeTrigger(triggerNode)
end
```

### Alternativ: Distanz-basiert (kein Trigger-Node nötig)

```lua
-- In onUpdate prüfen (alle 500ms für Performance)
function MeinSpec:_scanForObjects()
    local cx, cy, cz = getWorldTranslation(self.rootNode)
    local radius = 2.5

    for _, vehicle in pairs(g_currentMission.vehicles) do
        local vx, vy, vz = getWorldTranslation(vehicle.rootNode)
        local dist = MathUtil.vector3Length(cx-vx, cy-vy, cz-vz)
        if dist <= radius then
            -- Fahrzeug in Reichweite
        end
    end
end
```

---

## 22. i18n Übersetzungen

### de.xml / en.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<l10n>
    <text name="meinText_name">
        <text>Mein Text auf Deutsch</text>
    </text>
    <text name="ui_format">
        <text>Wert: %.1f kW</text>        <!-- string.format-kompatibel -->
    </text>
</l10n>
```

### In Lua verwenden

```lua
-- Einfacher Text
g_i18n:getText("meinText_name")

-- Mit Formatierung
string.format(g_i18n:getText("ui_format"), 22.5)

-- In XML-Referenz (Store-Daten)
-- <name>$l10n_meinText_name</name>    -- Dollar + l10n_ + Key
```

---

## 23. Bekannte Fallstricke

| Problem | Ursache | Lösung |
|---|---|---|
| `spec` ist `nil` in Methoden | `self.spec_name` nicht in `onLoad` initialisiert | Immer `local spec = {}; self.spec_name = spec` als erstes |
| Node-Funktionen schlagen fehl | Node-ID ist `nil` oder ungültig | Vor jedem `getWorldTranslation` etc. prüfen: `if node ~= nil then` |
| Netzwerk-Desync | Server ändert Wert, Client bekommt es nicht | `raiseDirtyFlags` + `onWriteUpdateStream` implementieren |
| Feststellbremse-Prüfung fehlt | FS25 hat kein `parkingBrakeIsSet` in Drivable | Fallback: `math.abs(vehicle:getLastSpeed()) < 0.5` |
| Placeable erscheint nicht im Shop | Falscher Pfad in `storeItem` oder `type`-Name | Pfad relativ zum Mod-Root, `type="ModName.typName"` |
| `onFinalizePlacement` nicht aufgerufen | `rootNode` in `onLoad` noch nicht gültig | World-Position erst in `onFinalizePlacement` abfragen |
| Globaler Manager ist `nil` | Script lädt bevor erstes Objekt erstellt wurde | Guard: `if g_meineGlobal == nil then g_meineGlobal = {} end` |
| Trigger feuert nicht | Collision Mask falsch oder Trigger-Flag vergessen | In GIANTS Editor: Trigger ✓ + Mask 255 |
| Memory-Leak bei Sounds/Overlays | `deleteSample` / `deleteImage` vergessen | Immer in `onDelete` aufräumen |
| `superFunc` ist nil | `registerOverwrittenFunction` statt `registerFunction` vergessen | Prüfen ob die Originalfunktion existiert |
| XML-Savegame-Schema fehlt | Eigener Pfad nicht in `xmlSchemaSavegame` registriert | Separates Schema-Register in `initSpecialization()` |
| `g_currentMission.vehicles` ist leer | Zugriff zu früh (vor Karten-Load) | Nur in Event-Callbacks oder `onUpdate` zugreifen |

---

## 24. Komplettes Spec-Grundgerüst

Fertige Vorlage zum Kopieren:

```lua
-- MeinSpec.lua  –  Vehicle Specialization Grundgerüst für FS25
-- Füge zur modDesc.xml hinzu:
--   <specialization name="meinSpec" className="MeinSpec" filename="scripts/specializations/MeinSpec.lua"/>

MeinSpec = {}
local MeinSpec_mt = Class(MeinSpec)

-- ── Voraussetzungen ──────────────────────────────────────────────────────────
function MeinSpec.prerequisitesPresent(specializations)
    return SpecializationUtil.hasSpecialization(Motorized, specializations)
end

-- ── Schemas ──────────────────────────────────────────────────────────────────
function MeinSpec.initSpecialization()
    local schema = Vehicle.xmlSchema
    schema:setXMLSpecializationType("MeinSpec")
    schema:register(XMLValueType.FLOAT, "vehicle.meinSpec#wert", "Mein Wert", 100)
    schema:setXMLSpecializationType()

    Vehicle.xmlSchemaSavegame:register(XMLValueType.FLOAT,
        "vehicles.vehicle(?).meinSpec#wert", "Gespeicherter Wert")
end

-- ── Funktionen & Events ──────────────────────────────────────────────────────
function MeinSpec.registerFunctions(vehicleType)
    SpecializationUtil.registerFunction(vehicleType, "getWert", MeinSpec.getWert)
    SpecializationUtil.registerFunction(vehicleType, "setWert", MeinSpec.setWert)
end

function MeinSpec.registerEventListeners(vehicleType)
    SpecializationUtil.registerEventListener(vehicleType, "onLoad",              MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onSaveToXMLFile",     MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onDelete",            MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onUpdate",            MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onDraw",              MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onReadStream",        MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onWriteStream",       MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onReadUpdateStream",  MeinSpec)
    SpecializationUtil.registerEventListener(vehicleType, "onWriteUpdateStream", MeinSpec)
end

-- ── Lifecycle ────────────────────────────────────────────────────────────────
function MeinSpec:onLoad(savegame)
    local spec         = {}
    self.spec_meinSpec = spec

    spec.wert      = self.xmlFile:getValue("vehicle.meinSpec#wert", 100)
    spec.dirtyFlag = self:getNextDirtyFlag()

    if savegame ~= nil then
        spec.wert = savegame.xmlFile:getValue(savegame.key .. ".meinSpec#wert", spec.wert)
    end
end

function MeinSpec:onSaveToXMLFile(xmlFile, key, usedModNames)
    xmlFile:setValue(key .. ".meinSpec#wert", self.spec_meinSpec.wert)
end

function MeinSpec:onDelete()
    -- Aufräumen: Trigger, Sounds, Overlays
end

-- ── Update ───────────────────────────────────────────────────────────────────
function MeinSpec:onUpdate(dt, isActiveForInput, isActiveForInputIgnoreSelection, isSynchronized)
    local spec = self.spec_meinSpec

    if self.isServer then
        -- Server-Logik
        -- spec.wert = spec.wert - dt * 0.001

        self:raiseDirtyFlags(spec.dirtyFlag)
    end
end

-- ── HUD ──────────────────────────────────────────────────────────────────────
function MeinSpec:onDraw()
    if not self.isActiveForInputIgnoreSelection then return end
    local spec = self.spec_meinSpec

    renderOverlay(nil, 0.02, 0.15, 0.10, 0.018, 0, 0, 0, 0.65)
    setTextAlignment(RenderText.ALIGN_CENTER)
    setTextColor(1, 1, 1, 1)
    renderText(0.07, 0.173, 0.013, string.format("%.0f", spec.wert))
end

-- ── Netzwerk ─────────────────────────────────────────────────────────────────
function MeinSpec:onWriteStream(streamId, connection)
    streamWriteFloat32(streamId, self.spec_meinSpec.wert)
end

function MeinSpec:onReadStream(streamId, connection)
    self.spec_meinSpec.wert = streamReadFloat32(streamId)
end

function MeinSpec:onWriteUpdateStream(streamId, connection, dirtyMask)
    if streamWriteBool(streamId, bitAND(dirtyMask, self.spec_meinSpec.dirtyFlag) ~= 0) then
        streamWriteFloat32(streamId, self.spec_meinSpec.wert)
    end
end

function MeinSpec:onReadUpdateStream(streamId, timestamp, connection)
    if streamReadBool(streamId) then
        self.spec_meinSpec.wert = streamReadFloat32(streamId)
    end
end

-- ── Öffentliche API ───────────────────────────────────────────────────────────
function MeinSpec:getWert()
    return self.spec_meinSpec.wert
end

function MeinSpec:setWert(wert)
    self.spec_meinSpec.wert = MathUtil.clamp(wert, 0, 100)
    if self.isServer then
        self:raiseDirtyFlags(self.spec_meinSpec.dirtyFlag)
    end
end
```

---

## 25. Map Modding – Grundstruktur

### Ordnerstruktur einer FS25-Karte

```
MeineKarte/
├── modDesc.xml
├── icon_MeineKarte.dds
├── map/
│   ├── map.i3d                       ← Haupt-Szene der Karte
│   ├── map.i3d.shapes                ← Geometrie-Daten (binär)
│   ├── map.xml                       ← Karten-Metadaten (Startpunkt, Limits)
│   ├── mapDE_environment.xml         ← Wetter, Jahreszeiten, Licht
│   ├── mapDE_sun.xml                 ← Sonnenparameter
│   ├── mapDE_items.xml               ← Vorplatzierte Objekte (Fahrzeuge, Gebäude)
│   ├── mapDE_vehicles.xml            ← Startfahrzeuge der Karte
│   ├── mapDE_placeables.xml          ← Vorplatzierte Placeables
│   ├── mapDE_trainSystem.xml         ← Zugsystem (falls vorhanden)
│   ├── terrain/
│   │   ├── mapDE_dem.png             ← Höhenkarte (16-bit Graustufen, 4096×4096)
│   │   ├── mapDE_infoLayer.grle      ← Spiellogik-Schicht (Felder, Gras, Straßen …)
│   │   └── mapDE_weight_*.png        ← Terrain-Texturen (Bodentypen)
│   ├── data/
│   │   ├── mapDE_densityMap_fruits.gdm  ← Frucht-Belegung
│   │   ├── mapDE_densityMap_ground.gdm  ← Boden-Typen
│   │   └── mapDE_fieldGroundSystem.xml
│   └── missions/
│       └── mapDE_missions.xml
├── sounds/
└── i18n/
```

### map.xml – wichtigste Felder

```xml
<?xml version="1.0" encoding="utf-8"?>
<map>
    <!-- Größe der Karte in Metern (typisch 2048 oder 4096) -->
    <width>2048</width>
    <height>2048</height>

    <!-- Höhenkarte-Parameter -->
    <heightScale>255</heightScale>         <!-- max. Höhe in Metern -->
    <maxTerrain>255</maxTerrain>

    <!-- Startposition des Spielers (Welt-Koordinaten) -->
    <startPoint posX="1024" posZ="1024"/>

    <!-- Spielgrenzen (Kaufbare Fläche) -->
    <boundingBox minX="-1024" minZ="-1024" maxX="1024" maxZ="1024"/>

    <!-- Frucht-Typen der Karte (referenziert mapDE_fruitTypes.xml) -->
    <fruitTypes filename="map/data/mapDE_fruitTypes.xml"/>
</map>
```

### modDesc.xml für eine Karte

```xml
<modDesc descVersion="72">
    <maps>
        <map id="meineKarte"
             className="MapDE"
             filename="map/map.i3d">
            <title>
                <de>Meine Karte</de>
                <en>My Map</en>
            </title>
            <description>
                <de>Beschreibung</de>
                <en>Description</en>
            </description>
            <!-- Vorschaubild im Karten-Auswahlscreen -->
            <imageFilename>icon_MeineKarte.dds</imageFilename>
        </map>
    </maps>
</modDesc>
```

### Terrain infoLayer – Bit-Masken (mapDE_infoLayer.grle)

| Kanal | Bedeutung | Typ |
|---|---|---|
| `fieldGround` | Felder, Pflugzustand, Düngung | DensityMap |
| `groundType` | Straße, Schotter, Wasser | DensityMap |
| `paintLayer` | Texturgewichtungen | DensityMap |
| `navigationMap` | Befahrbare Fläche (AI) | DensityMap |

### Werkzeuge

| Tool | Zweck |
|---|---|
| **maps4fs** (`maps4fs.xyz`) | Echtworld-Karte generieren aus OpenStreetMap + SRTM; liefert Höhenkarte, Straßennetz, Terrain |
| **METools für GE10** | GE10-Skripte für Terrain-Höhe, Splines, Objekt-Platzierung |
| **GIANTS Editor v10** | Terrain-Malen, Objekte platzieren, i3d-Exporter |
| **Blender + GIANTS Exporter** | 3D-Assets → i3d-Export |

**Tipp:** Der `dataS`-Ordner der FS25-Installation enthält die Vanilla-Karte (`mapDE`) als vollständiges Referenzbeispiel.

---

## 26. Production Chains

### XML-Struktur (placeable XML)

```xml
<?xml version="1.0" encoding="utf-8"?>
<placeable type="productionPoint">
    <storeData>
        <name>$l10n_meineProduktion</name>
        <price>250000</price>
        <category>PRODUCTION</category>
        <specs>
            <capacityFillTypes>Weizen Gerste Raps</capacityFillTypes>
        </specs>
    </storeData>

    <filename>placeables/meineProduktion/meineProduktion.i3d</filename>

    <!-- ── Produktions-Definition ── -->
    <productionPoint>
        <!-- Lager-Kapazitäten je Fülltyp -->
        <storages>
            <storage fillTypeCategories="GRAINTANK" capacity="100000"/>
            <storage fillTypeCategories="PALLETS"   capacity="500"/>
        </storages>

        <!-- Produktionsprozesse (kann mehrere geben) -->
        <productions>
            <production id="mein_prozess"
                        name="$l10n_prozessName"
                        params=""
                        cyclesPerHour="100"
                        costsPerActiveHour="10">
                <!-- Eingangsgüter -->
                <inputs>
                    <input fillType="WHEAT" amount="100"/>
                    <input fillType="WATER" amount="50"/>
                </inputs>
                <!-- Ausgangsgüter -->
                <outputs>
                    <output fillType="FLOUR"  amount="80" sellDirectly="false"/>
                    <!-- sellDirectly="true" → direkt verkauft, kein Lager nötig -->
                    <output fillType="CHAFF"  amount="20" sellDirectly="true"/>
                </outputs>
            </production>
        </productions>

        <!-- Verkaufspunkte (optionale Palette) -->
        <palletSpawner spawnOffset="0 0 2" maxNumPallets="10"/>
    </productionPoint>
</placeable>
```

### Wichtige Felder

| Attribut | Bedeutung |
|---|---|
| `cyclesPerHour` | Zyklen pro Spielstunde (1 Spielstunde ≈ 1 Minute Echtzeit) |
| `costsPerActiveHour` | Betriebskosten pro aktiver Spielstunde |
| `sellDirectly` | Output wird sofort zum Marktpreis verkauft (kein Lagerbedarf) |
| `fillTypeCategories` | Gruppenname (z.B. `GRAINTANK`, `PALLETS`) statt einzelner Fülltypen |

### Funktionale Placeables ohne GIANTS Toolkit

Ab FS25 hat sich `mapboundID` zu `uniqueId` geändert:

```xml
<!-- In mapDE_placeables.xml oder über In-Game-Platzierung gespeichert -->
<placeable
    filename="placeables/meineProduktion/meineProduktion.xml"
    uniqueId="meineProduktion_01"
    posX="100" posY="0" posZ="200"
    rotX="0"   rotY="0" rotZ="0"/>
```

### Lua: ProductionPoint-Spec abfragen

```lua
-- Auf ProductionPoint-Spec zugreifen
local spec = placeable.spec_productionPoint

-- Aktiven Prozess abfragen
local activeProductions = spec.activeProductions   -- table

-- Lagerstand eines Fülltyps
local fillLevel = placeable:getFillUnitFillLevel(fillUnitIndex)

-- Produktionsrate abfragen
local rate = spec:getCyclesPerHour("mein_prozess")
```

---

## 27. ModHub Einreichung

### Namenskonvention

| Element | Regel |
|---|---|
| Mod-Ordner / ZIP | `FS25_MeinModName` (Prefix `FS25_` Pflicht) |
| Interne IDs | Namespace voranstellen: `MeinMod.meineSpec` |
| Fahrzeug-XML | `meinFahrzeug.xml` (keine Leerzeichen, keine Sonderzeichen) |

### Pflichtfelder in modDesc.xml

```xml
<modDesc descVersion="72">
    <author>Dein Name</author>
    <version>1.0.0.0</version>        <!-- Major.Minor.Patch.Build, nach SemVer -->
    <title><de>...</de><en>...</en></title>
    <description><de>...</de><en>...</en></description>
    <iconFilename>icon_MeinMod.dds</iconFilename>
    <multiplayer supported="true"/>   <!-- oder false -->
</modDesc>
```

### Icon-Anforderungen

| Eigenschaft | Wert |
|---|---|
| Format | DDS (BC1/DXT1 oder BC3/DXT5 mit Alpha) |
| Größe | **256 × 256 px** |
| Dateiname | `icon_[ModName].dds` |
| Inhalt | Mod-Vorschaubild (wird im ModHub und im Spiel angezeigt) |

### Store-Vorschaubild

| Eigenschaft | Wert |
|---|---|
| Format | DDS |
| Größe | **512 × 256 px** |
| Verweis in XML | `<image>placeables/meinObjekt/store.dds</image>` |

### Versions-Konventionen

```
1.0.0.0  → Major.Minor.Patch.Build
           Major: Breaking Changes / Neu-Release
           Minor: Neue Features, abwärtskompatibel
           Patch: Bugfixes
           Build: interne Build-Nummer
```

### Pre-Upload-Checkliste

- [ ] Mod in `FS25_ModName.zip` gepackt (Ordner direkt in ZIP, kein Unter-Ordner)
- [ ] `modDesc.xml` mit korrektem `descVersion="72"` vorhanden
- [ ] Icon `256×256 px` als DDS vorhanden
- [ ] `multiplayer supported` korrekt gesetzt
- [ ] Kein sensibles Material (fremde Assets ohne Genehmigung)
- [ ] Im Spiel ohne Fehler in `log.txt` getestet
- [ ] GIANTS Modding Guidelines gelesen: `forum.giants-software.com/viewtopic.php?t=209169`

---

## 28. Referenz-Mods auf GitHub

Gut strukturierte Open-Source-Mods als Lernressource:

| Mod | URL | Was man lernt |
|---|---|---|
| **Courseplay FS25** | `github.com/Courseplay/Courseplay_FS25` | Komplexe Mod-Architektur, Event-System, GitHub-Actions-CI, Lua-Tests |
| **UniversalAutoload** | `github.com/loki79uk/FS25_UniversalAutoload` | Saubere Specialization-Implementierung, XML-Konfiguration, Savegame |
| **FS25 Lua Scripting** (dataS) | `github.com/Dukefarming/FS25-lua-scripting` | Spiel-interne Implementierung aller Specializations, Events, Utils |
| **FS25 Lua API** (Stubs) | `github.com/MyGameSteamOfficial/fs25-lua-api` | API-Stubs für IDE-Autocompletion (VSCode / IntelliJ) |
| **FS25 ModTemplate** | `github.com/Jos-Modding/FS25_ModTemplate` | ModHub-konformes Starter-Template |

### IDE-Setup für Lua-Autocompletion (VSCode)

```jsonc
// .vscode/settings.json
{
    "Lua.workspace.library": [
        "/Pfad/zu/fs25-lua-api"    // github.com/MyGameSteamOfficial/fs25-lua-api
    ],
    "Lua.diagnostics.globals": [
        "g_currentMission",
        "g_i18n",
        "g_soundManager",
        "g_inputBinding",
        "SpecializationUtil",
        "MathUtil",
        "XMLValueType",
        "RenderText",
        "InputAction"
    ]
}
```

Empfohlene VSCode-Erweiterung: **sumneko.lua** (Lua Language Server).

---

---

## 29. Mod Sync Server API (Pre-Join)

> **Quelle:** [github.com/spliffz/FS25-Mod-Sync-Server](https://github.com/spliffz/FS25-Mod-Sync-Server)  
> Selbst gehosteter PHP/Apache-Webserver, der Clients erlaubt, Mods **vor dem Beitreten** eines Multiplayer-Servers zu synchronisieren.

### Überblick

```
Client                          Mod-Sync-Server
  │                                    │
  │── GET /ajax.php?getModList ────────▶│  komplette Mod-Liste holen
  │◀─ JSON [name, hash, size] ─────────│
  │                                    │
  │── GET /ajax.php?checkMod&… ────────▶│  einzelnen Mod prüfen
  │◀─ JSON {update: 0|1} ──────────────│
  │                                    │
  │── HTTP-Download /mods/FS25_X.zip ──▶│  fehlende/veraltete Mods laden
  │◀─ ZIP-Datei ────────────────────────│
  │                                    │
  └── Spiel beitreten ◀────────────────┘
```

---

### Öffentliche Endpunkte (kein Login nötig)

#### `GET /ajax.php?getModList`

Gibt die vollständige Liste aller Mods auf dem Server zurück.

**Anfrage:**
```
GET http://<server>/ajax.php?getModList
```

**Antwort (JSON-Array):**
```json
[
  ["FS25_ModName",     "a3f1c2d4e5b6...", 1572864],
  ["FS25_AndererMod",  "9b8e7a6c5d4f...", 3145728]
]
```

| Index | Typ | Inhalt |
|---|---|---|
| `[0]` | string | Dateiname des Mods (ohne Pfad) |
| `[1]` | string | MD5-Hash der Datei |
| `[2]` | int | Dateigröße in Bytes |

---

#### `GET /ajax.php?checkMod`

Prüft ob ein bestimmter Mod auf dem Server vorhanden ist und ob die lokale Version aktuell ist.

**Anfrage:**
```
GET http://<server>/ajax.php?checkMod&modname=FS25_Mod&modhash=abc123&size=1234567
```

| Parameter | Pflicht | Beschreibung |
|---|---|---|
| `modname` | ✓ | Dateiname des Mods |
| `modhash` | ✓ | MD5-Hash der lokalen Datei |
| `size` | ✓ | Dateigröße der lokalen Datei in Bytes |

**Antwort – Mod aktuell (`update: 0`):**
```json
{
  "name":   "FS25_Mod",
  "hash":   "abc123",
  "update": 0,
  "size":   "1234567",
  "msg":    "ok"
}
```

**Antwort – Update verfügbar (`update: 1`):**
```json
{
  "name":   "FS25_Mod",
  "hash":   "neuerhash456",
  "update": 1,
  "size":   "1234999",
  "msg":    "ok"
}
```

**Antwort – Mod nicht auf Server vorhanden:**
```json
{
  "msg": "none"
}
```

---

### URL-Alias (via .htaccess)

```
/ajax/getModList  →  /ajax.php?getModList
```

---

### Typischer Pre-Join-Ablauf

```lua
-- Pseudocode: Mod-Sync-Logik vor Spielbeitritt

-- 1. Komplette Serverliste holen
local serverMods = HTTP.get(serverUrl .. "/ajax.php?getModList")
-- serverMods = { {"FS25_Mod", "hash", size}, ... }

-- 2. Mit lokalen Mods abgleichen
for _, serverMod in ipairs(serverMods) do
    local name, serverHash, serverSize = serverMod[1], serverMod[2], serverMod[3]
    local localHash = getLocalMD5(modsFolder .. name)

    if localHash == nil then
        -- Mod fehlt lokal → herunterladen
        downloadMod(serverUrl .. "/mods/" .. name)
    elseif localHash ~= serverHash then
        -- Mod veraltet → Einzelcheck + ggf. neu laden
        local check = HTTP.get(serverUrl ..
            "/ajax.php?checkMod&modname=" .. name ..
            "&modhash=" .. localHash ..
            "&size=" .. getLocalSize(name))
        if check.update == 1 then
            downloadMod(serverUrl .. "/mods/" .. name)
        end
    end
end

-- 3. Spiel beitreten
```

---

### Technische Details

| Eigenschaft | Wert |
|---|---|
| Protokoll | HTTP (PHP 8.2 / Apache) |
| Antwortformat | JSON |
| CORS | `Access-Control-Allow-Origin: *` (alle Origins erlaubt) |
| Hash-Algorithmus | MD5 |
| Datei-Endung | `.zip` (Mods liegen im `/mods/`-Ordner) |
| Authentifizierung | Keine für öffentliche Endpunkte |
| Datenbank | MySQL/MariaDB, Tabelle `mods` |

---

### Datenbankschema (`mods`-Tabelle)

```sql
CREATE TABLE mods (
    id    INT AUTO_INCREMENT PRIMARY KEY,
    name  VARCHAR(255) NOT NULL,   -- Dateiname
    hash  VARCHAR(255) NOT NULL,   -- MD5-Hash
    size  INT                      -- Dateigröße in Bytes
);
```

---

### Admin-Endpunkte (Login erforderlich)

Alle Admin-Endpunkte erfordern eine gültige Session und sind über `POST /acp/ajax.php` erreichbar:

| `?request=` | Funktion |
|---|---|
| `getModList` | Mod-Liste im Admin-Panel anzeigen |
| `reindex` | Alle Mods im `/mods/`-Ordner neu indizieren (MD5 neu berechnen) |
| `dtm` | Mod löschen (Parameter: `mid`) |
| `importMods` | Mods von GPortal-FTP importieren |
| `importMods_saveChanges` | FTP-Zugangsdaten speichern |
| `changePass` | Admin-Passwort ändern (SHA-256) |

Upload über `POST /acp/upload.php` (multipart/form-data).

---

### Selbst hosten (Docker)

Das Repository enthält eine `docker-compose.yml`:

```bash
git clone https://github.com/spliffz/FS25-Mod-Sync-Server
cd FS25-Mod-Sync-Server
docker compose up -d
# → Webinterface: http://localhost:80
# → Setup: http://localhost/INSTALL/install.php
```

---

*Letzte Aktualisierung: 2026-05-28 | FS25 v1.15.0.0*  
*Quellen: [GDN LuaDoc](https://gdn.giants-software.com/documentation_scripting_fs25.php) · [GDN Doku](https://gdn.giants-software.com/documentation.php) · [GDN Videos](https://gdn.giants-software.com/videoTutorials2.php) · [Community LuaDoc](https://umbraprior.github.io/FS25-Community-LUADOC/) · [FS25 Lua Scripting](https://github.com/Dukefarming/FS25-lua-scripting) · [FS25 Lua API](https://github.com/MyGameSteamOfficial/fs25-lua-api) · [Scripting-Buch (Open Access)](https://library.oapen.org/handle/20.500.12657/86976) · [AlfaMods](https://alfamods.no/faq/fs25-modding/) · [maps4fs](https://maps4fs.xyz) · [FS25 Mod Sync Server](https://github.com/spliffz/FS25-Mod-Sync-Server)*
