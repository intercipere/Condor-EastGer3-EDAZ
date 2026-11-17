# EastGermany3 / EDAZ Schönhagen

Arbeitsstand für eine erneuerte Condor-3-Landschaft rund um den Flugplatz EDAZ (Schönhagen), aufbauend auf EastGermany2. Der Stand ist ein Arbeitsstand, noch kein Release.

## Inhalt

1. `PROJEKT.md`: Ziel, Erkenntnisse, Dateiformate, Datenquellen und offene Punkte.
2. `Werkzeuge/`: Python-Skripte für Luftbild-Texturen, Gelände, Wald, Flugplatzflächen, Gebäude (LoD2), Fassaden, Dörfer (Autogen) und die Baumtextur.
3. `Vorschau.bat`: öffnet eine Blender-Vorschau der eingebauten Dateien.

## Nicht enthalten

1. Keine Landschaftsdateien von EastGermany2 und keine Condor-Dateien. Wer das nutzen will, braucht eine eigene, unveränderte EastGermany2 und Condor 3.
2. Keine Rohdaten. Die Brandenburger Geodaten (DOP20, DGM1, bDOM, LoD2) lädt man vom Server der LGB, siehe `PROJEKT.md`.
3. Die Skripte enthalten noch feste Pfade (`C:/Spiele/Condor3/...`). Sie müssen für eine andere Installation angepasst werden.

## Quellen und Rechte

1. Geodaten: © GeoBasis-DE/LGB, dl-de/by-2-0. Der genaue Wortlaut der Quellenangabe wird noch geprüft.
2. EastGermany2: Felix Hupfeld (Drh), Michel Bernard und Claude Simonneaud. Eine Rückfrage beim Autor wegen der Weitergabe ist offen.
3. Eine Lizenz für diesen Code und die Doku ist noch nicht festgelegt.
