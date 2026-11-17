# TODO: was noch zu tun ist

Sammelliste für alle, die mitarbeiten wollen. Stand: 17.11.2026. Details zu den einzelnen Punkten stehen in `PROJEKT.md` (Bauliste, Offene Punkte, Erkenntnisse) und `ARBEITSABLAUF.md` (Reihenfolge der Skripte). Wer einen Punkt anfängt, trägt das bitte hier ein (Name und Datum), damit niemand doppelt arbeitet.

## 1. Verteilung und Rechtliches

1. **Anfrage an die Autoren von EastGermany2** (Felix Hupfeld, Michel Bernard, Claude Simonneaud), ob die Weitergabe der geänderten Teile in Ordnung ist. Läuft, noch keine Antwort.
2. **Quellenangabe für die Brandenburger Geodaten:** Wortlaut in der BbgGeoNutzV prüfen. Bisher „© GeoBasis-DE/LGB, dl-de/by-2-0“.
3. **Lizenz für Code und Doku festlegen.** Ohne Lizenz darf niemand den Code verwenden oder weitergeben.
4. **Release-Paket bauen:** Ein Zip mit den eigenen, selbst erzeugten Dateien (Texturen, Höhenkacheln, Flugplatz- und Autogen-Dateien, Waldkarten) und einer Prüfsummenliste, als GitHub-Release.
5. **Installer schreiben:** Legt `EastGermany3` neben `EastGermany2` an (Hardlinks statt 16 GB Kopie, ungetestet), spielt das Release ein und erzeugt die abgeleiteten Dateien (`.obj`, `.apt`, Hash-Zeilen in `.fha` und `.oha`, `Drevesa.dds`) aus der EG2 des Mitglieds. Prüft vorher, dass die EG2 unverändert ist.

## 2. Lücken im Repo

1. Download-Skript für die LGB-Kacheln (Dateinamen siehe `ARBEITSABLAUF.md`).
2. Die OSM-Auszüge (`Daten\OSM`) ins Repo, mit ODbL-Quellenangabe.
3. Die Hilfsskripte zur Steuerung von Landscape Editor und ObjectEditor ins Repo.
4. Klären und beschreiben, wie `Airports\Schoenhagen.grs` (40 MB) entsteht.
5. **Fehler beheben:** `objektliste.py` Zeile 54 erwartet zwei Rückgabewerte von `lies_gebaeude`, es sind drei.
6. Link zum Condor Landscape Toolkit in `ARBEITSABLAUF.md` ergänzen.
7. Veralteten Aufrufhinweis im Kopf von `patch_textur.py` korrigieren.
8. Prüfen, was `scenerycheck3.exe` zur Landschaft meldet, und das Ergebnis festhalten.

## 3. In Condor ansehen und prüfen

1. Neue Hallen, Hangartore und den Geflügelhof Löwendorf samt Landwerk Ahrensdorf im Flug ansehen (Maße der Silos und Lüfter sind geschätzt).
2. Prüfen, ob Condor eigene Meshes (z. B. aus DOM und DOP20) als Landschaftsobjekte akzeptiert, und ob die Dateiprüfung bei Online-Flügen geänderte Spieldateien bemängelt.

## 4. Inhalte für EDAZ

Die vollständige Liste steht im Abschnitt „Bauliste EDAZ“ in `PROJEKT.md`. Offen sind unter anderem:

1. Weitere Gebäudearten: Tower-Komplex in Kalksandstein, Hangar P1 mit Rolltoren.
2. Denkmäler (Do 27 im Zebra-Look, Schulgleiter auf Mast) und Umgebung (Baumreihen, Teich, Zäune).
3. Eigene Fotos vom Platz (frontal, bedeckter Himmel). Sie sind die beste Quelle für Fassaden.
4. Fassaden näher an die Fotos bringen, zum Beispiel Hangar-Tore über die ganze Seite. Eigene Texturen dafür, keine aus fremden Quellen.
5. Vorfeldmarkierungen ins Bodenluftbild.

## 5. Karte erweitern

1. Ring aus scharfen Luftbildern und neuem Wald um EDAZ (3 × 3 Patches), damit die Bildflug- und Patchkanten vom Platz wegrücken.
2. Datenaufbereitung automatisieren (laden, nach UTM umrechnen, Farben angleichen, Patches erzeugen), dann Kachel für Kachel erneuern. Die Patches stehen bisher fest in den Skripten.

## 6. Ideen, bisher zurückgestellt

1. **Mehr Wolkenvariation:** Es gibt nur 9 Wolkenmodelle und einen Textur-Atlas, und Wolken wirken nur global (nicht pro Landschaft). Siehe „Wolken und Shader“ in `PROJEKT.md`.
2. **Shader-Änderungen:** Condor lädt `Effects\fx\<Name>.fx` im Klartext, der Originalquelltext liegt aber nicht vor.
