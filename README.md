# EastGermany3 / EDAZ Schönhagen

Arbeitsstand für eine erneuerte Condor-3-Landschaft rund um den Flugplatz EDAZ (Schönhagen), aufbauend auf EastGermany2. Der Stand ist ein Arbeitsstand, noch kein Release.

## Inhalt

1. `ARBEITSABLAUF.md`: Voraussetzungen, Einrichtung, Reihenfolge der Skripte und Regeln fürs Mitarbeiten, auch für KI-Agents. Hier anfangen.
2. `TODO.md`: was noch zu tun ist, nach Themen sortiert.
3. `PROJEKT.md`: Ziel, Erkenntnisse, Dateiformate, Datenquellen und offene Punkte.
4. `Werkzeuge/`: Python-Skripte für Luftbild-Texturen, Gelände, Wald, Flugplatzflächen, Gebäude (LoD2), Fassaden, Dörfer (Autogen) und die Baumtextur. In `Werkzeuge/steuerung/` liegen Skripte, die den Landscape Editor über Win32-Nachrichten bedienen und den Autogen-Export samt Hash erledigen, dazu PowerShell-Helfer für Screenshots und Tasten in Condor.
5. `requirements.txt` (Python-Pakete) und `Vorschau.bat` (öffnet eine Blender-Vorschau der eingebauten Dateien).

Der Condor-Pfad steht in `Werkzeuge/config.py` (Standard `C:/Spiele/Condor3`, änderbar über die Umgebungsvariable `CONDOR_DIR`).

## Nicht enthalten

1. Keine Landschaftsdateien von EastGermany2 und keine Condor-Dateien. Wer das nutzen will, braucht eine eigene, unveränderte EastGermany2 und Condor 3.
2. Keine Rohdaten. Die Brandenburger Geodaten (DOP20, DGM1, bDOM, LoD2) lädt man vom Server der LGB, siehe `PROJEKT.md`.

## Quellen und Rechte

1. Geodaten: © GeoBasis-DE/LGB, dl-de/by-2-0. Der genaue Wortlaut der Quellenangabe wird noch geprüft.
2. EastGermany2: Felix Hupfeld (Drh), Michel Bernard und Claude Simonneaud.
3. Die gebaute Landschaft wird derzeit nur intern genutzt. Vor einer Weitergabe sind eine Rückfrage bei den Autoren von EastGermany2, der genaue Wortlaut der Quellenangabe und eine Lizenz für Code und Doku nötig.
