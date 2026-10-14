# Mod „Valheim Welt Live“ (BepInEx, inoffiziell)

Begleit-Mod für den Welt-Viewer. Er liest die Welt **direkt aus dem laufenden Spiel bzw. Server** und zeigt sie live im Browser:
3D-Karte, Kisten, Portale, Erbauer und **Spielerpositionen in Echtzeit**. Auf dem zweiten Bildschirm einfach `http://localhost:8686` öffnen.

> Status: erste Version, kompiliert, aber noch **nicht im echten Spiel getestet**. Die Spiel-Schnittstellen (ZDOMan, ZDO, ZNet) können sich mit Updates ändern. Falls etwas nicht klappt: `BepInEx/LogOutput.log` ansehen.

## Bauen

Voraussetzungen: Valheim mit **BepInEx** (z. B. „BepInExPack Valheim“ von Thunderstore) und das **.NET SDK** (8 oder neuer).

```bat
mod\build.bat "C:\Program Files (x86)\Steam\steamapps\common\Valheim" install
```

`build.bat` baut die DLL, packt die Webseite dazu und kopiert alles nach `Valheim\BepInEx\plugins\ValheimWeltLive\`. Ohne das Wort `install` wird nur das Paket unter `mod\paket\` erstellt.

## Lokal (Einzelspieler oder selbst gehostet)

1. Mod installieren (siehe oben), Valheim starten, Welt laden.
2. Im Browser `http://localhost:8686` öffnen. Die Karte verbindet sich automatisch.
3. Die Welt wird alle 30 s neu eingelesen, Spielerpositionen jede Sekunde.

Auch die GitHub-Seite kann sich verbinden: *„Live mit dem Mod verbinden“* → `http://localhost:8686` (Chrome/Edge/Firefox erlauben das für localhost).

## Server (z. B. Nitrado)

Auf dem Server wird meist kein Port für eine Webseite freigegeben. Deshalb schreibt der Mod die Welt als Datei, und das **Valheim-Sync-Programm** holt sie per FTP und lädt sie zu GitHub hoch.

1. Prüfen, ob der Server BepInEx unterstützt. Bei Nitrado gibt es dafür meist eine Mod-/BepInEx-Option. Dann den Ordner `ValheimWeltLive` per FTP nach `BepInEx/plugins/` laden.
2. Server einmal starten. Danach in `BepInEx/config/de.valheimweltviewer.live.cfg` folgende Werte setzen:
   ```
   [Datei]
   WriteDumpFile = true
   [Datenschutz]
   StripSteamIds = true
   [Daten]
   WorldIntervalSeconds = 120
   [Web]
   Enabled = false
   ```
3. Der Mod schreibt jetzt alle 2 Minuten `BepInEx/config/ValheimWeltLive/welt.vwdump`. Valheim-Sync findet die Datei automatisch.

## Einstellungen (`BepInEx/config/de.valheimweltviewer.live.cfg`)

| Abschnitt | Schlüssel | Standard | Bedeutung |
|---|---|---|---|
| Web | Enabled | true | eingebaute Webseite/Schnittstelle |
| Web | Port | 8686 | `http://localhost:PORT/` |
| Web | BindAll | false | auch aus dem LAN erreichbar (Windows: ggf. Adminrechte) |
| Daten | WorldIntervalSeconds | 30 | Welt neu einlesen (min. 10) |
| Daten | PlayerIntervalSeconds | 1 | Spielerpositionen |
| Datei | WriteDumpFile | false | Datei für FTP-Sync schreiben |
| Datei | DumpPath | leer | Standard `BepInEx/config/ValheimWeltLive/welt.vwdump` |
| Datenschutz | StripSteamIds | true | Steam-IDs nicht ausgeben |

## Schnittstelle

| Adresse | Inhalt |
|---|---|
| `/` | Viewer (Ordner `web`) |
| `/api/info` | `{ world, zdos, dumpTime, … }` |
| `/api/players` | `[{ name, x, y, z }]` |
| `/api/world.vwdump` | ganze Welt, Format VWD1 (gzip) – siehe `docs/FORMAT.md` |

## Bekannte Grenze

Valheim 1.0 speichert die Welt in Chunk-Dateien. Lädt das Spiel ungenutzte Gebiete erst bei Bedarf, enthält der Live-Dump nur die geladenen Bereiche. Das ist noch ungeprüft; die Zahl der Objekte steht unter `/api/info` (eure Welt hat ca. 600.000).
