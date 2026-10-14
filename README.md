# Valheim Welt-Viewer (inoffiziell)

**➜ Zur Seite: https://blackwarrior309.github.io/Valheim-welt/**

Verwandelt eine Valheim-Welt (ab 1.0) in eine **3D-Karte der ganzen erkundeten Welt** mit allen Gebäuden, **Kisteninhalten**, Portalen, Erbauern und Bauplänen. Die Karte lässt sich als **eine Datei teilen** oder **live** anzeigen, über einen Server-Abgleich oder den Begleit-Mod.

## So geht's

1. Seite öffnen: https://blackwarrior309.github.io/Valheim-welt/
2. Weltdateien reinziehen: `<Welt>.fwl2`, `<Welt>.db2` und die `.chunk`-Dateien, am einfachsten alles als ZIP.
   Lokale Welten liegen unter `%USERPROFILE%\AppData\LocalLow\IronGate\Valheim\worlds_local`.
3. Die Umwandlung läuft komplett im Browser, es wird nichts hochgeladen.
4. Ansehen: Karte, 3D (ganze Welt oder einzelne Basen, Flugmodus mit WASD), Kisten durchsuchen, Welt & Spieler.
5. Teilen: oben **„Teilen-Datei (.html)“** speichern und verschicken. Die Datei öffnet sich per Doppelklick im Browser, auch offline.

| Aufruf | Was passiert |
|---|---|
| https://blackwarrior309.github.io/Valheim-welt/ | Startseite: Welt reinziehen → umwandeln → ansehen → Teilen-Datei speichern |
| https://blackwarrior309.github.io/Valheim-welt/?src=live/welt.vwelt&refresh=300 | Live-Welt aus dem Repo, lädt alle 5 Min. neu (sobald Valheim-Sync läuft) |
| http://localhost:8686 | dieselbe Seite, ausgeliefert vom Mod, live aus dem laufenden Spiel |
| `<Welt>.html` | Teilen-Datei: Viewer + Daten in einer Datei |

## Feedback

Fehler, falsch dargestellte Bauteile oder Wünsche bitte als Issue melden: https://github.com/Blackwarrior309/Valheim-welt/issues
Am hilfreichsten sind Screenshots, einmal im Spiel und einmal im Viewer aus ähnlichem Winkel.

## Bestandteile

```
index.html, css/, js/, lib/, plans/   die Webseite (statisch, GitHub Pages)
  js/convert.js                       Spielstand/Mod-Dump → .vwelt (Browser und Node)
  js/viewer.js                        Anzeige: Karte, 3D, Kisten, Welt & Spieler
  js/i18n.js                          Sprache: Deutsch/Englisch (Umschalter oben rechts, ?lang=en)
  plans/                              Baupläne (Grasland-Halle)
sync/                                 Valheim-Sync: FTP (Nitrado) → umwandeln → hier ins Repo hochladen
mod/                                  Begleit-Mod „Valheim Welt Live“ (BepInEx, C#-Quellcode + build.bat)
tools/                                convert.cjs (Kommandozeile), Generatoren für Namenslisten
docs/FORMAT.md                        Dateiformate .vwelt, .vwplan, VWD1
```

## Live-Karte vom Server (Nitrado)

`sync/` auf den PC kopieren und `Valheim-Sync.bat` starten, Anleitung in `sync/LIESMICH.txt`. Das Programm holt die Welt per FTP, wandelt sie um und lädt `live/welt.vwelt` in dieses Repo (`"repo": "Blackwarrior309/Valheim-welt"`). Dafür braucht es nur Node.js und einen GitHub-Token mit Schreibrecht auf dieses Repo, kein Git.

**Achtung:** Das Repo ist öffentlich, also ist auch die Live-Welt für jeden mit Link abrufbar. Valheim-Sync lässt Steam-IDs deshalb standardmäßig weg (`stripSteamIds: true`).

## Live aus dem eigenen Spiel (Mod)

Siehe `mod/README.md`: Mod bauen und installieren, Valheim starten, dann http://localhost:8686 auf dem zweiten Bildschirm öffnen. Die Welt wird alle 30 s gelesen, die Spieler jede Sekunde.

## Leistung & Grafik

Im 3D-Menü unter **„Leistung & Grafik“**: Schatten (Standard: aus; wenn an, nur im Stillstand berechnet und im Flugmodus immer aus), Grafik (Standard: Natur-Texturen für Holz, Stein und Stroh, auch bei Bauplänen; alternativ „Einfach“ oder „Blöcke“), Natur-Dichte, Auflösung und Sichtweite. Der Browser merkt sich die Einstellungen.

## Lokal ausprobieren

```bash
python3 -m http.server 8000          # dann http://localhost:8000
node tools/convert.cjs <weltordner|zip> welt.vwelt [--strip-ids]
```

## Grenzen

* **Weltformat:** nur Valheim 1.0 (`.fwl2`, `.db2`, `.chunk`). Alte Welten (`.fwl` + `.db`) einmal im aktuellen Spiel laden und speichern.
* **Gelände:** wird aus Bäumen, Steinen und Pflanzen geschätzt, nicht aus dem Seed nachgebaut.
* **Bauteile:** vereinfachte Formen. Seltene Teile erscheinen als „unbekannt“; Namenslisten in `tools/*.py`, `python3 tools/gen_data.py` erzeugt `js/data.js`.
* **Mod:** noch nicht im echten Spiel getestet.

Inoffizielles Fan-Projekt, nicht mit Iron Gate verbunden. Valheim ist eine Marke von Iron Gate AB. Ohne Spielgrafiken. MIT-Lizenz (`LICENSE`); enthalten sind three.js (MIT) und fflate (MIT).
