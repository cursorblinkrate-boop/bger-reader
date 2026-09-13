# BGE-Reader

**Barrierefreie Bundesgerichtsentscheinde.** / **Accessibility tool for Swiss Federal Supreme Court decisions.**

## Unterstützte Seiten:
https://search.bger.ch/*
https://relevancy.bger.ch/*
http://relevancy.bger.ch/*

## Schriftarten
- Atkinson Hyperlegible Next (https://www.brailleinstitute.org) 
- opendyslexic (https://opendyslexic.org)
- Serif, Sanf Serif


## Anleitung 
Skript läuft über die kostenlose Browser-Erweiterung **Tampermonkey**
(Firefox, Chrome, Edge, Brave).

1. **Tampermonkey installieren:**
   - Firefox: https://addons.mozilla.org/de/firefox/addon/tampermonkey/
   - Chrome/Edge/Brave: https://www.tampermonkey.net/
2. Tampermonkey-Symbol in der Symbolleiste anklicken → **„Neues Skript erstellen"**
3. reinklicken (cmd + a) ganzen Text markieren und löschen.
4. Inhalt von [`bger-reader.user.js`](bger-reader.user.js) in die Zwischenablage kopieren
5. Inhalt bei Tampermonkey einfügen. (cmd + c / cmd + v)
6. **Strg+S** / **Cmd+S**– fertig!
7. Darauf achten, dass Tampermonkey aktiv ist (grünes Häkchen) -> Skript startet automatisch beim Besuch von search.bger.ch/* 

# Gallerie

### *default*
<img width="816" height="941" alt="naked" src="https://github.com/user-attachments/assets/4ffec15f-a941-4a63-aa73-fd9b71125062" />

### *brakets on*
<img width="1332" height="832" alt="3_auf" src="https://github.com/user-attachments/assets/33a28668-8232-40c8-9865-70fe680bb06f" />

### *brakets off*
<img width="1334" height="942" alt="1_closed" src="https://github.com/user-attachments/assets/a3585080-3d2f-4302-a2cb-b4f817e4561d" />
<img width="1005" height="684" alt="3_zu" src="https://github.com/user-attachments/assets/5045d4a8-147f-4b56-b79f-3090543b388c" />
<img width="1325" height="931" alt="2_zu" src="https://github.com/user-attachments/assets/2c29d089-55d0-45c1-90e7-c712d80a18e7" />
