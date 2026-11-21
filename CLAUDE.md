# Siegerschluck – Trinkspiel-App

Partyspiel für ein Handy, das in der Runde rumgeht. Kernregel: **Der Gewinner trinkt.**
Alles steckt in `index.html` (HTML + CSS + JS, keine Abhängigkeiten außer Google Fonts).

## Spielregeln (so ist es umgesetzt)
- 2–8 Spieler, gespielt wird bis 5/10/15 Punkte.
- Jede Runde: Minispiel ansagen → (Krone wählt Macht) → Einsätze → Sabotage-Reveal → Minispiel → Auswertung.
- Einsätze: Sicher (+1 Punkt / 1 Schluck), All-In (+3 / 2), Saboteur (spielt nicht mit, verteilt einen Nachteil; erst ab 3 Spielern).
- Sabotage-Karten: Doppelt trinken, Bremse (je Minispiel anders), Schwache Hand, Punktklau (−1 Punkt).
- Krone & Fluch: Führender hat Krone und vor jeder Runde eine Macht (All-In erzwingen, Punkte tauschen, Gratis-Sabotage). Gewinnt er als Kronenträger, kostet das ×2, danach ×3 – bis er überholt wird.
- Heißes Eisen: Zwangsrunde (20 % ab Runde 2), Schlucke ×2.
- Max. 10 Schlucke pro Runde. Gesamtsieger trinkt Sieger-Drink und legt eine Königs-Regel fest (wird gespeichert).

## Code-Aufbau in index.html
- `STRATS`, `EFFECTS`, `POWERS`, `QUESTIONS`, `KING_IDEAS`, `MAX_DRINKS`, `HOT_CHANCE` – Inhalte & Balancing ganz oben.
- `GAMES` – Minispiel-Registry. Neues Spiel = neuer Eintrag mit `name`, `desc`, `slow` (Bedeutung der Bremse), optional `hot:true`, und `play(teilnehmerIds, done)`. Am Ende `done([gewinnerIds])` aufrufen (leeres Array = keiner gewinnt).
- Helfer im Spiel: `show(html)`, `later(fn, ms)` (Timer werden beim Screenwechsel automatisch gestoppt), `has(id,'slow')`, `nm(id)`, `SFX.*`, `buzz()`.
- `reveal()` rechnet Punkte und Schlucke inkl. aller Multiplikatoren.

## Testen
`npx serve .` und auf dem Handy im selben WLAN öffnen, oder `index.html` direkt im Browser.
Texte kurz halten, Buttons groß (mind. 44 px), dunkles Design beibehalten.

## PWA
- `manifest.json`, `sw.js`, `icons/` (Quelle: `icons/icon.svg`; PNGs per `qlmanage -t -s 512` + `sips` erzeugt).
- Nach Änderungen an `index.html` oder Icons in `sw.js` die `VERSION` hochzählen.
- Service Worker läuft nur über HTTPS oder `localhost` – über `http://<LAN-IP>` ist die App installierbar, aber nicht offline-fähig.

## Deploy
- Live: https://goldy2200.github.io/siegerschluck/ (GitHub Pages, Branch `main`, Root). Repo: `Goldy2200/siegerschluck`.
- Update: `VERSION` in `sw.js` hochzählen → committen → `git push`. Pages baut in ~1 Min neu.
