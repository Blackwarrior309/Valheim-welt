# Valheim Welt-Viewer (inoffiziell)

Verwandelt eine Valheim-Welt (ab 1.0) in eine **3D-Karte der ganzen erkundeten Welt** mit allen Gebäuden, **Kisteninhalten**, Portalen, Erbauern und Bauplänen. Die Karte lässt sich als **eine Datei teilen** oder **live** anzeigen, über einen Server-Abgleich oder den Begleit-Mod.

**Eine Seite für alles.** `index.html` ist Umwandler und Viewer zugleich:

| Aufruf | Was passiert |
|---|---|
| `https://<name>.github.io/<repo>/` | Startseite: Welt reinziehen → umwandeln → ansehen → Teilen-Datei speichern. Liegt eine Live-Welt im Repo, steht oben der Kasten „Live-Welt“. |
| `…/?src=live/welt.vwelt&refresh=300` | öffnet direkt die Live-Welt und lädt alle 5 Min. neu (das verlinkt der Kasten) |
| `http://localhost:8686` | dieselbe Seite, ausgeliefert vom Mod, live aus dem laufenden Spiel |
| `<Welt>.html` (Teilen-Datei) | Viewer + Daten in einer Datei, per Doppelklick, auch offline |

## Bestandteile

```
index.html, css/, js/, lib/, plans/   die Webseite (statisch, für GitHub Pages)
  js/convert.js                       Spielstand/Mod-Dump → .vwelt   (Browser und Node, derselbe Code)
  js/viewer.js                        Anzeige: Karte, 3D, Kisten, Welt & Spieler
  plans/                              mitgelieferte Baupläne (Grasland-Halle)
sync/                                 Valheim-Sync: FTP (Nitrado) → umwandeln → GitHub hochladen (Windows/Mac/Linux, Node.js)
mod/                                  Begleit-Mod „Valheim Welt Live“ (BepInEx, C#-Quellcode + build.bat)
tools/                                convert.cjs (Kommandozeile), Generatoren für Namenslisten
docs/FORMAT.md                        Dateiformate .vwelt, .vwplan, VWD1
```

## Online stellen (GitHub Pages) – z. B. um Feedback zu sammeln

1. Kostenloses Konto auf github.com anlegen bzw. anmelden.
2. Oben rechts **+ → New repository**, Name z. B. `valheim-welt`, **Public**, *Create*.
3. Auf der leeren Repo-Seite **„uploading an existing file“** wählen. Den *Inhalt* dieses Ordners hineinziehen, also `index.html`, `css`, `js`, `lib`, `plans`, `docs`, `tools`, `sync`, `mod`, `README.md`, `LICENSE`. Dann *Commit changes*.
4. **Settings → Pages → Source: Deploy from a branch → Branch `main` / `(root)` → Save.**
5. Nach 1–2 Minuten erscheint dort der Link `https://<name>.github.io/valheim-welt/`. Diesen Link teilt ihr zum Ausprobieren.

Hinweise:
* Mit einem kostenlosen Konto muss das Repo **öffentlich** sein, damit Pages läuft. Dann ist auch eine hochgeladene Live-Welt öffentlich abrufbar. Valheim-Sync lässt Steam-IDs darum standardmäßig weg (`stripSteamIds: true`).
* Für eine reine Feedback-Seite reicht die Webseite. `sync/` und `mod/` müssen nicht online sein.
* Feedback einsammeln: unter *Settings → Features* „Issues“ aktiv lassen und auf der Seite auf das Repo verlinken.

## Live-Karte vom Server (Nitrado)

`sync/` auf den PC kopieren und `Valheim-Sync.bat` starten. Die Anleitung steht in `sync/LIESMICH.txt`. Das Programm holt die Welt per FTP, wandelt sie um, speichert eine Teilen-Datei und lädt `live/welt.vwelt` per GitHub-API ins Repo. Git muss dafür nicht installiert sein, nur Node.js.

## Live aus dem eigenen Spiel (Mod)

`mod/README.md`: Mod bauen und installieren, Valheim starten, dann `http://localhost:8686` auf dem zweiten Bildschirm öffnen. Die Welt wird alle 30 s gelesen, die Spieler jede Sekunde.

## Lokal ausprobieren

```bash
python3 -m http.server 8000          # dann http://localhost:8000
node tools/convert.cjs <weltordner|zip> welt.vwelt [--strip-ids]
```

## Leistung & Grafik

Im 3D-Menü unter **„Leistung & Grafik“**: Schatten (Standard: aus; wenn an, werden sie nur im Stillstand berechnet und sind im Flugmodus immer aus), Grafik (Standard: Natur-Texturen für Holz, Stein und Stroh, auch bei Bauplänen; alternativ „Einfach“ oder „Blöcke“), Natur-Dichte, Auflösung und Sichtweite. Die Einstellungen merkt sich der Browser. Gezeichnet wird nur, wenn sich etwas bewegt, stillstehende Ansichten kosten also kaum Leistung.

## Grenzen

* **Gelände:** wird aus Bäumen, Steinen und Pflanzen geschätzt. Ein Nachbau des Weltgenerators aus dem Seed fehlt noch.
* **Bauteile:** sind vereinfachte Blöcke. Seltene Teile und Items erscheinen als „unbekannt“; die Namenslisten stehen in `tools/*.py`, `python3 tools/gen_data.py` erzeugt daraus `js/data.js`.
* **Format:** Valheim 1.0 (`.fwl2`, `.db2`, `.chunk`). Spiel-Updates können Anpassungen nötig machen. Der Mod ist noch nicht im echten Spiel getestet.

Inoffizielles Fan-Projekt, nicht mit Iron Gate verbunden. Valheim ist eine Marke von Iron Gate AB. Kostenlos, ohne Spielgrafiken. MIT-Lizenz (`LICENSE`); enthalten sind three.js (MIT) und fflate (MIT).
