# MVgrabber
MEDIA FINDER
MVgrabber / MEDIA FINDER v14 - README
Dieses Projekt startet einen Playwright-gesteuerten Firefox mit persistentem Profil und scannt
Netzwerk-Responses nach Media-Stream-URLs (HLS/DASH/MP4). Danach laedt es die gefundenen
Streams mit ffmpeg in Downloads_Serie/.
Hinweis (wichtig): Nutze das Tool nur fuer Inhalte, fuer die du Rechte hast (eigene Uploads, eigene
Streams, freigegebene Medien). Dein Firefox-Profil kann Cookies/Logins enthalten - bitte nicht teilen.
1) Welche Dateien brauche ich wirklich?
Muss im Projekt bleiben
• main.py (Programm)
• install.py (Setup)
• requirements.txt (Python-Abhaengigkeiten)
• firefox_profile/ (Ordner - darf leer sein, aber soll existieren) -> Add-ons & Einstellungen bleiben
hier gespeichert
• Downloads_Serie/ (Ausgabeordner fuer Downloads)
Kann weg / wird automatisch neu erzeugt
• Cache-Inhalte im Profil (werden beim Start neu erzeugt)
Wichtig: Wenn du firefox_profile/ loeschst, sind Add-ons/Settings weg und muessen neu
eingerichtet werden.
2) Installation (einmalig)
2.1 System-Paket: ffmpeg (Pflicht)
Auf Debian/Kali:
sudo apt update
sudo apt install -y ffmpeg
2.2 Python Setup: install.py (empfohlen)
Im Projektordner:
python3 install.py
install.py macht:
• pip install -r requirements.txt
• python -m playwright install firefox (laedt Playwright-Firefox)
• optional: Linux Playwright-Dependencies (falls noetig)
• prueft, ob ffmpeg vorhanden ist
3) Erste Einrichtung: Firefox-Fenster + Add-ons (nur 1x noetig)
Warum reicht 1x?Das Programm nutzt ein persistentes Firefox-Profil im Ordner firefox_profile/. Dadurch bleiben
Add-ons, Toolbar-/UI-Einstellungen, Cookies/Logins und weitere Browser-Settings erhalten.
Alles bleibt beim naechsten Start erhalten, solange du firefox_profile/ nicht loeschst.
4) Add-ons installieren (AdGuard / Ghostery / uBlock Origin)
1) Starte das Programm (nicht headless, Standard).
2) Es oeffnet sich ein Firefox-Fenster.
3) Installiere diese Add-ons:
• AdGuard
• Ghostery
• uBlock Origin
Browser-UI einstellen (nur 1x)
• Im Firefox-Fenster: Rechtsklick oben -> Menueleiste anzeigen
• Bookmarks/Toolbar: Bookmarks Toolbar -> Always show (immer anzeigen)
Danach: Firefox-Fenster minimieren (nicht schliessen!).
Wenn du das Firefox-Fenster schliesst, beendet Playwright die Sitzung.
5) Programm ausfuehren (immer so, inkl. GDB)
Du willst es grundsaetzlich mit GDB laufen lassen (Debug + Backtrace):
gdb -batch -ex "run" -ex "bt" --args python3 main.py
6) Bedienung im Programm (Ablauf)
• Im Terminal eingeben: Staffel-URL (q = Ende): <deine URL>
• Programm scannt Episoden: Scanne Episoden und meldet die Anzahl
• Bei Alle laden? (j/n): mit j beantworten
Ergebnis / Downloads
Dateien landen im Projektordner unter Downloads_Serie/.
7) Was passiert intern? (Kurz-Analyse)
7.1 Firefox Start (persistentes Profil)
• Playwright startet Firefox als persistent context
• Profil: firefox_profile/
• Fenster: z.B. 1000x700
• Dadurch bleiben Add-ons & UI-Settings dauerhaft erhalten
7.2 Stream-Sniffing
• Das Script lauscht auf response Events• Es filtert Medien-URLs (z.B. .m3u8, .ts, .mp4, manifest.mpd, Segments/Chunks)
• Unnuetzes wird gefiltert (Assets, Bilder, CSS/JS)
7.3 Auto-Klick Logik
• Alle 5 Sekunden wird automatisch in die Fenstermitte geklickt, damit Consent/Overlay/Player
reagiert
7.4 Download
• Download ueber ffmpeg
• Ausgabe nach Downloads_Serie/
8) Haeufige Probleme & Fixes
Add-ons sind weg
Ursache: firefox_profile/ geloescht oder anderer Profilpfad genutzt.
Fix: firefox_profile/ behalten (dann bleiben Add-ons dauerhaft).
Playwright-Firefox startet nicht
python -m playwright install firefox
python -m playwright install-deps firefox
# falls Linux libs fehlen
ffmpeg fehlt
sudo apt install -y ffmpeg
9) Profil komplett zuruecksetzen (nur wenn du 'frisch' willst)
Achtung: Dadurch gehen Add-ons/Settings verloren.
rm -rf firefox_profile
Beim naechsten Start: Profil wird neu erstellt, Add-ons muessen neu installiert werden.
Erstellt am 2026-02-26
