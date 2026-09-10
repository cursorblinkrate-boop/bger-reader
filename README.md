# BGer Reader 📖

**An independent accessibility and focus layer for Swiss Federal Supreme Court decisions**
*(search.bger.ch – unabhängiger Prototyp, nicht mit dem Schweizerischen Bundesgericht verbunden.)*

Ein Browser-Werkzeug, das Entscheide des Bundesgerichts besser lesbar macht –
besonders für Menschen mit Lese-Einschränkungen, ADHS oder Sehschwäche.

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Tests: 49](https://img.shields.io/badge/tests-49%20checks-brightgreen.svg)
![Dependencies: none](https://img.shields.io/badge/dependencies-none-brightgreen.svg)

## Warum dieses Projekt?

Entscheide auf search.bger.ch erscheinen in einer festen, kleinen Typografie.
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

## Features

- 🔤 Typografie: Schriftgrösse, Schriftart (Serif/Sans), Schriftstärke, Zeilenabstand, Buchstaben- und Wortabstand, maximale Zeilenlänge
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

1. Einen Entscheid auf [search.bger.ch](https://search.bger.ch) öffnen
2. Oben rechts erscheint **„📖 BGer Reader"** (Titel anklicken minimiert das Panel)
3. **„Lesemodus einschalten"** aktivieren und alles nach Belieben anpassen

## Designentscheidungen

| Entscheidung | Begründung |
|---|---|
| Kein `innerHTML`-/Regex-Parsing über das ganze Dokument | zerstört Links und Formatierung; stattdessen TreeWalker + Bracket-Stack + `Range.extractContents()` |
| Konservative Heuristik statt perfekter Erkennung | Jahreszahlen, Aktenzeichen und Gesetzesartikel enthalten Zahlen; lieber zu wenig automatisch einklappen – jede Stelle bleibt manuell aufklappbar |
| Styles nur per Klasse + CSS-Variablen auf `div.paraatf`/`div.eit` | die Absatzstruktur bleibt unversehrt; beim Ausschalten ist alles wie vorher |
| Panel im Shadow DOM | Schutz in beide Richtungen gegen das alte Seiten-CSS |
| Kernlogik (`window.BGerReader`) ohne Tampermonkey-Abhängigkeit | gleicher Code kann später als Firefox-/Chromium-Erweiterung verpackt werden |
| Nur `search.bger.ch`, `@grant none` | minimale Berechtigung; kein Zugriff auf Seiteninhalte seitens des Skripts über privilegierte APIs |

## Tests

49 automatisierte Prüfungen (jsdom) gegen eine **echte heruntergeladene Entscheidseite**
(BGE 152 IV 1) sowie synthetische Fixtures: verschachtelte/unbalancierte Klammern,
Links in Klammern, Jahreszahlen, Aktenzeichen, Gesetzesartikel, Reversibilität
(Roundtrip stellt den Original-DOM exakt wieder her).

```bash
npm install jsdom
curl -s "https://search.bger.ch/ext/eurospider/live/de/php/clir/http/index.php?highlight_docid=atf%3A%2F%2F152-IV-1%3Ade&lang=de&type=show_document" -o /tmp/bger_test.html
node test/test-runner.js
```

## Bekannte Grenzen (Version 1)

- Nur HTML-Entscheide auf search.bger.ch, nur Deutsch – PDFs, Französisch/Italienisch und ältere Seitentypen sind Version 2
- Die Literaturerkennung ist **heuristisch**: einzelne Literaturklammern werden nicht erkannt, in seltenen Fällen kann eine inhaltliche Bemerkung eingeklappt werden. Das ist beabsichtigt – Erkennung ist reversibel und nie destruktiv
- Keine Nutzertests durchgeführt bisher – willkommen als Beitrag!

## Haftungsausschluss

*Independent prototype, not affiliated with the Swiss Federal Supreme Court.*
Alle Änderungen passieren nur lokal im Browser; es werden keine Daten übertragen.

## Lizenz

[MIT](LICENSE) – frei verwendbar und weitergebbar.
