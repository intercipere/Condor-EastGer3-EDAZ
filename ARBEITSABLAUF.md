# Arbeitsablauf EastGermany3 / EDAZ

Diese Datei beschreibt, was man installieren muss und in welcher Reihenfolge die Skripte laufen. Sie richtet sich an Menschen und an KI-Agents, die an der Landschaft mitarbeiten. Hintergrund, Dateiformate und Messwerte stehen in `PROJEKT.md`.

Stand: 17.11.2026. Die Reihenfolge ist aus den Abhängigkeiten der Skripte abgeleitet und wurde nicht als Ganzes auf einem frischen Rechner durchlaufen. Wo etwas ungeprüft ist, steht es dabei.

## 1. Voraussetzungen

1. **Windows.** Getestet wurde unter Windows 11. Die Editoren des Condor-Toolkits laufen nur dort.
2. **Condor 3** mit dem **Landscape Toolkit** (`LandscapeEditor.exe`, `ObjectEditor.exe`, `scenerycheck3.exe`). Die Toolkit-Programme müssen im Condor-Ordner liegen, sonst starten sie nicht. Den Download-Link bitte beim Condor-Team erfragen, er steht noch nicht in dieser Doku.
3. **Eine unveränderte Landschaft `EastGermany2`** in `Landscapes\`. Sie wird nie verändert, nur als Vorlage gelesen.
4. **Python 3.12** und die Pakete aus `requirements.txt`: `pip install -r requirements.txt`.
5. **Blender 5.3** nur für `Vorschau.bat` und `kiefer_blender.py`.
6. **Platz:** Die Rohdaten brauchen etwa 6 GB (`Daten\LGB`), die Arbeitskopie `EastGermany3` etwa 16 GB, wenn man sie kopiert.

## 2. Einrichten

1. **Projekt holen:** Repo klonen, die Skripte liegen in `Werkzeuge\`.
2. **Condor-Pfad:** Die Skripte erwarten Condor unter `C:/Spiele/Condor3`. Anderer Ort: Umgebungsvariable `CONDOR_DIR` setzen, zum Beispiel `set CONDOR_DIR=D:/Games/Condor3`. Alle Pfade stehen in `Werkzeuge\config.py`.
3. **Arbeitskopie anlegen:** Den Ordner `Landscapes\EastGermany2` nach `Landscapes\EastGermany3` kopieren und alle Dateien `EastGermany2.*` im Hauptordner in `EastGermany3.*` umbenennen. Einmal in Condor laden und prüfen, dass die Landschaft startet. Ein Installer mit Hardlinks statt Kopie ist geplant, aber nicht gebaut.
4. **Rohdaten:** Es gibt noch kein Download-Skript. Die Dateien lädt man von https://data.geobasis-bb.de/geobasis/daten/ (Lizenz dl-de/by-2-0, Quellenangabe „© GeoBasis-DE/LGB, dl-de/by-2-0“) nach `Daten\LGB`:
   1. `dop\dop_33<Rechts>-<Hoch>.zip` (Luftbild), `dgm\dgm_...zip` (Gelände), `bdom\bdom_...zip` (Oberflächenmodell), `lod2\lod2_...zip` (Gebäude). Die Zahlen sind Rechtswert und Hochwert der 1-km-Kachel, zum Beispiel `dop_33368-5782.zip`.
   2. Abgedeckt sein muss Rechtswert 368–380, Hochwert 5782–5788 (UTM 33). Die Skripte entpacken selbst nach `dop_jpg`, `dgm_tif` und `bdom_tif`.
   3. `Daten\OSM\` enthält vier JSON-Auszüge von OpenStreetMap. Die Skripte lesen nur `edaz_aeroway.json` und `edaz_bauten.json`. `doerfer_landuse.json` und `places.json` liest kein Skript im Repo, sie sind Hilfsdaten. Die Dateien sind noch **nicht** im Repo, siehe Abschnitt 7.

## 3. Reihenfolge der Skripte

Alle Aufrufe im Ordner `Werkzeuge\`. Die Patches 40/39 und 39/39 (EDAZ und Ahrensdorf) stehen fest in den Skripten (`PATCHES`).

1. **Luftbild:** `python patch_textur.py <pixel> <ausgabeordner> <spalte,zeile> [<spalte,zeile> ...]`, zum Beispiel `python patch_textur.py 8192 ..\Daten\Arbeit 40,39 39,39`. Alle Patches müssen in derselben Reihe liegen. Das Skript liest die alten Texturen aus `EastGermany2`, schreibt `t4039_8192.png` und `t3939_8192.png` und dazu `mosaik.npy` und `mosaik_flug.npy`. Diese beiden Dateien müssen im Ordner `Daten\Arbeit` landen, weil `edaz_flaechen.py` und `waldkarte.py` sie dort lesen. Danach `python dds_dxt1.py <png> <dds>` und die DDS nach `EastGermany3\Textures\t<SS><ZZ>.dds` legen. (Der Aufruf im Kopf der Datei ist veraltet, maßgeblich ist `main()`.)
2. **Gelände:** Im Landscape Editor das 22,5-m-Gelände als OBJ exportieren (nach `Working\HeightMaps\22.5m`). Dann `python edaz_gelaende.py` (schreibt nach `...\22.5m\Modified`). Anschließend wandelt der Landscape Editor die OBJ in `.tr3f` um. Die `.tr3f` liegen in `HeightMaps\22.5m`, jede Datei mit 6-stelligem und 4-stelligem Namen (siehe `PROJEKT.md`, Stand EDAZ, Punkt 2).
3. **Wald:** `python waldkarte.py` (schreibt `ForestMaps\<SS><ZZ>.for`). Danach `LandscapeEditor.exe -hash EastGermany3`, das aktualisiert die Hashes in `.fha` und `.tha`.
4. **Flugplatzflächen:** `python edaz_flaechen.py` (liest Flugplatzpunkt, Höhe und Richtung aus `EastGermany3.apt`, schreibt `Daten\Arbeit\EDAZ\SchoenhagenG.c3d`, die Unter-Textur und `kontrolle_flaechen.png`). Die Dateien kommen nach `EastGermany3\Airports`. Flugplatzhöhe in der `.apt` laut `PROJEKT.md` setzen. Die Grasdatei `Airports\Schoenhagen.grs` (40 MB) erzeugt keines unserer Skripte. Wie sie entstanden ist, ist nicht dokumentiert (vermutlich ein Export im Editor), das muss noch geklärt werden.
5. **Flugplatzgebäude:** `python edaz_gebaeude.py` (schreibt `SchoenhagenO.c3d`, die Dach- und Wandtexturen und `kontrolle_daecher.png` nach `Daten\Arbeit\EDAZ`). Die Texturen kommen nach `Airports\Textures`.
6. **Objektliste:** `python objektliste.py --pruefen` prüft nur, ohne `--pruefen` schreibt es `EastGermany3.obj`. **Bekannter Fehler:** Das Skript bricht derzeit mit `ValueError: too many values to unpack` ab, weil `edaz_gebaeude.lies_gebaeude` inzwischen drei Werte liefert, `objektliste.py` Zeile 54 aber zwei erwartet. Vor der Nutzung beheben.
7. **Dörfer (Autogen):** `python doerfer_autogen.py` (schreibt `Working\Autogen\o<Patch>.obj/.mtl` und Texturen nach `Daten\Arbeit\Autogen`). Dann im Landscape Editor „File > Export autogen C3D from OBJ“ (die C3D nach `EastGermany3\Autogen`), die Texturen nach `Autogen\Textures` kopieren und „File > Export autogen hash (OHA)“ ausführen.
8. **Baumtextur:** Kiefer rendern mit `blender -b --python kiefer_blender.py -- <kiefer.png>`, dann `python baumtextur.py <kiefer.png> <Condor-Ordner>\Landscapes\EastGermany3\World\Textures\Drevesa.dds`. Das Ziel liegt im Landschaftsordner, nicht im globalen `World`. Das Skript liest die globale `<Condor-Ordner>\World\Textures\drevesa.dds` nur als Vorlage für den Laubbaum und ändert sie nicht, solange das Ziel ein anderer Pfad ist. Nie das Ziel auf die globale Datei legen.
9. **Fassadentexturen:** `python fassaden.py <ordner>` schreibt Beispielbilder zur Kontrolle. Die Gebäudeskripte rufen `fassaden.py` selbst auf.
10. **Vorschau:** `Vorschau.bat` baut eine Blender-Szene aus den eingebauten Dateien (Blender-Pfad in der Datei anpassen).

## 4. Prüfen

1. **Toolkit:** `scenerycheck3.exe` prüft die Landschaft. Ein Ergebnis dazu ist hier noch nicht festgehalten.
2. **In Condor:** Flug auf EDAZ starten. Zum Neuladen reicht „Flug beenden, im Flugplaner neu starten“, Condor muss nicht neu gestartet werden.
3. **Skriptkontrollen:** Die Gebäude- und Flächenskripte schreiben `kontrolle_*.png` nach `Daten\Arbeit\EDAZ`.
4. **Automatische Tests gibt es nicht.** Jede Änderung an einem Format oder einer Geometrie bitte gegen Originaldateien oder Messwerte prüfen, nicht nur gegen den eigenen Code.

## 5. Regeln für alle, die mitarbeiten

1. `EastGermany2` und die Dateien im Condor-Ordner bleiben unverändert. Gearbeitet wird nur in `EastGermany3`. Vor größeren Änderungen eine Sicherung anlegen.
2. Nach jeder Änderung an Wald oder Gelände `LandscapeEditor.exe -hash EastGermany3` ausführen, nach Autogen-Änderungen den OHA-Export. Condor prüft die Hashes auch im Freiflug.
3. Keine fremden Texturen oder Modelle verwenden, auch nicht aus FS2024-Screenshots. Erlaubt sind eigene Fotos, offene Daten mit Quellenangabe (LGB dl-de/by-2-0) und prozedural erzeugte Inhalte.
4. Keine Landschaftsdateien von EastGermany2, keine Condor-Dateien und keine Rohdaten ins Repo. Gebaute Ergebnisse gehören in ein Release, nicht in die Git-Historie (Dateien über 100 MB nimmt GitHub nicht).
5. Neue Erkenntnisse zu Dateiformaten mit Messwert in `PROJEKT.md` eintragen.

## 6. Hinweise für KI-Agents

1. Lies zuerst `PROJEKT.md` und dann diese Datei. Frage nach, bevor du Condor, den Landscape Editor oder den ObjectEditor startest, sie öffnen Fenster auf dem Rechner der Person.
2. Die Editoren lassen sich nur über Fensternachrichten und Mausklicks steuern, es gibt keine Kommandozeile außer `-hash`. Die Hilfsskripte dafür (PowerShell, Menü-IDs 23 und 14) liegen noch nicht im Repo.
3. Condor ignoriert simulierte Mausbewegungen, Tasten müssen als Scancodes gesendet werden (Details in `PROJEKT.md`, Erkenntnis 7).
4. Behaupte nicht, dass etwas funktioniert, ohne es ausgeführt zu haben. Prüfwerte aus Originaldateien oder Messungen herleiten, nicht aus dem getesteten Code.
5. Setze keine Pfade im Code fest, sondern nutze `config.py`.

## 7. Offene Punkte für die Doku

1. Download-Skript für die LGB-Daten und die OSM-Auszüge ins Repo.
2. Die Editor-Hilfsskripte ins Repo.
3. Ein Installer, der EastGermany3 per Hardlinks aus EastGermany2 anlegt und die eigenen Dateien aus einem Release einspielt.
4. Fehler in `objektliste.py` beheben.
5. Link zum Condor Landscape Toolkit ergänzen.
6. Herkunft von `Schoenhagen.grs` klären und beschreiben.
