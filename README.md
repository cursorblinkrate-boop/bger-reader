# BGer Reader 📖

**Accessibility and focus layer for Swiss Federal Supreme Court decisions**

Ein Browser-Addon zum Lesen von Bundesgerichtsentscheinden. 


![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Tests: 57](https://img.shields.io/badge/tests-57%20checks-brightgreen.svg)
![Dependencies: none](https://img.shields.io/badge/dependencies-none-brightgreen.svg)

## Warum dieses Projekt?

Entscheide auf den BGer-Webseiten erscheinen in einer festen, kleinen Typografie.
Klassische Reader-Funktionen (z. B. in Safari) lösen das Problem, indem sie den
Entscheid in einen einzigen Fliesstext verwandeln – die nummerierten Absätze,
die die argumentative Struktur tragen, gehen dabei verloren. Dieses Projekt
verändert die Seite deshalb **nicht strukturell**, sondern baut eine dünne
Accessibility-Schicht darüber: Absätze, Überschriften und Links bleiben
vollständig erhalten.

Die eigentliche Ingenieursarbeit steckt in den **Literaturklammern**:
mehrzeilige Nachweise wie `(ATF 143 IV 27 consid. 2.5; JEANNERET/GAUTIER, in: Commentaire romand …)`
unterbrechen beim ersten Durchlesen den Lesefluss. BGer Reader erkennt sie
**heuristisch und reversibel** – nichts wird gelöscht, alles bleibt mit einem
Klick verfügbar, beim Drucken wird der volle Text gezeigt.

## Unterstützte Seiten

| Seite | Inhalt | Absatz-Klassen |
|---|---|---|
| `search.bger.ch/…/clir/…` | BGE (amtliche Sammlung) | `div.paraatf` |
| `search.bger.ch/…/aza/…` | Weitere Urteile ab 2000 | `div.para` |
| `relevancy.bger.ch/…/clir/…` | ältere BGE-Ansicht | `div.paraatf` |

## Features

- 🔤 Typografie: Schriftgrösse, Schriftart (Serif/Sans), Schriftstärke, Zeilenabstand, Buchstaben- und Wortabstand, maximale Zeilenlänge
- ↔️ **Spaltenbreite einstellbar** (die Textkolonne zwischen den Haarlinien, Standard 625 px)
- ✂️ Silbentrennung (Sprache der Seite wird respektiert)
- 🎨 Farbschemata: Weiss, Sepia, Dunkel, Hoher Kontrast
- 📚 **Literaturklammern einklappbar** – zwei Modi:
  - *Nur wahrscheinliche Literaturhinweise* (Standard, konservative Heuristik: `S. 123`, `Rz. 45`, `in:`, `BGE/ATF …`, Autoren-Mehrfachnennungen, `ff.`)
  - *Alle Klammern ab Mindestlänge* (streng formal, Mindestlänge einstellbar)
- 🔗 **Links und Formatierungen innerhalb der Klammern bleiben erhalten** (Range-basiertes Einklappen, kein `innerHTML`-Parsing)
- ♿ Barrierearm: echte Buttons mit `aria-expanded` und Tastaturbedienung, sichtbarer Fokus, Disclosure-Pattern (W3C APG)
- 🖨️ Beim Drucken wird immer der vollständige Text angezeigt
- 💾 Einstellungen lokal gespeichert (`localStorage`) – kein Server, kein Tracking, keine externen Schriftarten
- 🧩 Panel in einem **Shadow DOM** – das Seiten-CSS kann es nicht zerstören (und umgekehrt)

## Installation (ca. 3 Minuten, keine Programmierkenntnisse nötig)

Das Werkzeug läuft über die kostenlose Browser-Erweiterung **Tampermonkey**
(Firefox, Chrome, Edge, Brave).

### Variante A: Ein-Klick-Installation (empfohlen)

1. **Tampermonkey installieren:**
   - Firefox: https://addons.mozilla.org/de/firefox/addon/tampermonkey/
   - Chrome/Edge/Brave: https://www.tampermonkey.net/
2. **Diesen Link anklicken** (Tampermonkey übernimmt automatisch):
   👉 https://raw.githubusercontent.com/cursorblinkrate-boop/bger-reader/main/bger-reader.user.js
3. Im Tampermonkey-Fenster auf **„Installieren"** klicken – fertig!

### Variante B: Manuell

1. Tampermonkey-Symbol in der Symbolleiste anklicken → **„Neues Skript erstellen"**
2. Vorlage löschen und den Inhalt von [`bger-reader.user.js`](bger-reader.user.js) einfügen
3. **Strg+S** / **Cmd+S** – fertig!

## Benutzung

1. Einen Entscheid auf einer der unterstützten Seiten öffnen
2. Oben rechts erscheint **„📖 BGer Reader"** (Titel anklicken minimiert das Panel)
3. **„Lesemodus einschalten"** aktivieren und alles nach Belieben anpassen



## Tests

57 automatisierte Prüfungen (jsdom) gegen **drei echte heruntergeladene Entscheidseiten**
(BGE 152 IV 1 / clir, Weiteres Urteil / aza, BGE 116 IA 359 / relevancy) sowie synthetische
Fixtures: verschachtelte/unbalancierte Klammern, Links in Klammern, Jahreszahlen,
Aktenzeichen, Gesetzesartikel, Reversibilität (Roundtrip stellt den Original-DOM exakt
wieder her) und die Spaltenbreiten-Steuerung.

```bash
npm install jsdom
# Fixtures laden (Befehle stehen im Kopfkommentar von test/test-runner.js)
node test/test-runner.js
```

## Roadmap / bekannte Grenzen

- **Version 2.2 (geplant):** Feintuning der Klammerklassifikation – Unterscheidung von
  reinen Literaturangaben (`Buch XYZ, S. 28`), Artikelverweisen (`Art. 22 ZGB`) und
  Klammerbemerkungen mit wesentlichem materiellem Inhalt (z. B. *nemo-tenetur*-Grundsatz),
  jeweils mit eigener Klapp-Strategie.
- **Idee (zu prüfen):** eigene Schriftdateien laden (z. B. Legasthenie-Schriften) –
  technisch machbar (CSS FontFace API), aufwendig in der Bedienung.
- Version 1 deckt nur HTML-Entscheide auf Deutsch ab – PDFs, Französisch/Italienisch
  und weitere Heuristiken sind spätere Versionen.
- Die Literaturerkennung ist **heuristisch**: einzelne Literaturklammern werden nicht
  erkannt, in seltenen Fällen kann eine inhaltliche Bemerkung eingeklappt werden.
  Das ist beabsichtigt – Erkennung ist reversibel und nie destruktiv.
- Keine Nutzertests durchgeführt bisher – willkommen als Beitrag!

## Haftungsausschluss

*Independent prototype, not affiliated with the Swiss Federal Supreme Court.*
Alle Änderungen passieren nur lokal im Browser; es werden keine Daten übertragen.

## Lizenz

[MIT](LICENSE) – frei verwendbar und weitergebbar.
