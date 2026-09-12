# BGE-Reader

**Ein Browser-Addon zum Lesen von Bundesgerichtsentscheinden.** 
**Accessibility layer for Swiss Federal Supreme Court decisions**


## Unterstützte Seiten

| Seite | Inhalt | Absatz-Klassen |
|---|---|---|
| `search.bger.ch/…/clir/…` | BGE (amtliche Sammlung) | `div.paraatf` |
| `search.bger.ch/…/aza/…` | Weitere Urteile ab 2000 | `div.para` |
| `relevancy.bger.ch/…/clir/…` | ältere BGE-Ansicht | `div.paraatf` |



--- work in progress



# Anleitung - BROWSER ADDON NEXT!!

Das winzige Skript läuft über die kostenlose Browser-Erweiterung **Tampermonkey**
(Firefox, Chrome, Edge, Brave).


## Variante A: BROWSER ADDON NEXT!!

1. **Tampermonkey installieren:**
   - Firefox: https://addons.mozilla.org/de/firefox/addon/tampermonkey/
   - Chrome/Edge/Brave: https://www.tampermonkey.net/
2. Tampermonkey-Symbol in der Symbolleiste anklicken → **„Neues Skript erstellen"**
3. Vorlage löschen und den Inhalt von [`bger-reader.user.js`](bger-reader.user.js) einfügen
4. **Strg+S** / **Cmd+S** – fertig!






## Features (geplant)
- 🔤 Typografie: Schriftgrösse, Schriftart (Serif/Sans), Schriftstärke, Zeilenabstand, Buchstaben- und Wortabstand, maximale Zeilenlänge
- ↔️ **Spaltenbreite einstellbar** (die Textkolonne zwischen den Haarlinien, Standard 625 px)
- ✂️ Silbentrennung (Sprache der Seite wird respektiert)
- 🎨 Farbschemata: Weiss, Sepia, Dunkel, Hoher Kontrast
- 📚 **Literaturklammern einklappbar** – zwei Modi:
  - wahrscheinliche Literaturhinweise* (Standard, konservative Heuristik: `S. 123`, `Rz. 45`, `in:`, `BGE/ATF …`, Autoren-Mehrfachnennungen, `ff.`)
  - *Alle Klammern ab Mindestlänge* (streng formal, Mindestlänge einstellbar)
- 🔗 **Links und Formatierungen innerhalb der Klammern bleiben erhalten** (Range-basiertes Einklappen, kein `innerHTML`-Parsing)
- ♿ Barrierearm: echte Buttons mit `aria-expanded` und Tastaturbedienung, sichtbarer Fokus, Disclosure-Pattern (W3C APG)
- 🖨️ Beim Drucken wird immer der vollständige Text angezeigt
- 💾 Einstellungen lokal gespeichert (`localStorage`) – kein Server, kein Tracking, keine externen Schriftarten
- 🧩 Panel in einem **Shadow DOM** – das Seiten-CSS kann es nicht zerstören (und umgekehrt)







## Lizenz

[MIT](LICENSE) – frei verwendbar und weitergebbar.
