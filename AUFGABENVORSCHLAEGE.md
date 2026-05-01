# Aufgabenvorschläge aus Code-Review

## 1) Tippfehler korrigieren
**Beobachtung:** Im Prozess-Abschnitt steht „إلى كنتي كاتقول …“. Das wirkt wie ein Tipp-/Schreibfehler; idiomatischer ist sehr wahrscheinlich „إيلا كنتي كاتقول …“.

**Aufgabe:**
- Korrigiere den Text im Absatz unter „4 خطوات بسيطة“.
- Prüfe zusätzlich ähnliche kurze Formulierungen auf konsistente Schreibweise im gesamten Dokument.

**Akzeptanzkriterien:**
- Die korrigierte Formulierung wird im Browser korrekt angezeigt.
- Kein unbeabsichtigter Textaustausch an anderen Stellen.

---

## 2) Programmierfehler beheben
**Beobachtung:** Am Dateiende gibt es verschachtelte/duplizierte `<script>`-Tags (`<script><script>`) und DOM-Text-Manipulation per `document.body.innerHTML = ...`, wodurch Event-Handler verloren gehen oder XSS-/Stabilitätsprobleme entstehen können.

**Aufgabe:**
- Entferne die fehlerhafte doppelte Script-Struktur.
- Ersetze die globale `innerHTML`-Ersetzung durch gezielte DOM-Updates (z. B. nur konkrete Zielknoten via `textContent`).

**Akzeptanzkriterien:**
- Keine doppelten/ungültigen Script-Tags mehr.
- CTA-Klicktracking (`onclick="fbq('track','Lead')"`) funktioniert weiterhin.
- Counter/Countdown funktionieren unverändert.

---

## 3) Kommentar-/Doku-Unstimmigkeit korrigieren
**Beobachtung:** Der Kommentar „Pixel-exact from official brand guide (PDF)“ suggeriert streng exakte Markenimplementierung, während gleichzeitig Placeholder (`YOUR_PIXEL_ID`, `YOUR_VIDEO_URL`) enthalten sind und Teile dynamisch nachträglich per Script ersetzt werden.

**Aufgabe:**
- Überarbeite den Kommentar im CSS-Header in eine realistische, überprüfbare Beschreibung.
- Ergänze im Kopfbereich eine kurze „Setup“-Notiz für erforderliche Platzhalterwerte.

**Akzeptanzkriterien:**
- Kommentare widersprechen dem tatsächlichen Zustand der Datei nicht mehr.
- Neue Teammitglieder verstehen sofort, welche Werte vor Deployment gesetzt werden müssen.

---

## 4) Test verbessern
**Beobachtung:** Es gibt keine automatisierte Prüfung für HTML-Strukturfehler oder riskante DOM-Manipulationen.

**Aufgabe:**
- Führe einen einfachen statischen Check ein (z. B. via HTMLHint oder eigenes Node/Python-Skript), der mindestens prüft:
  1. keine doppelten `<script>`-Starttags direkt hintereinander,
  2. keine Verwendung von `document.body.innerHTML =` im Produktions-HTML,
  3. Pflicht-Placeholder (`YOUR_PIXEL_ID`, `YOUR_VIDEO_URL`) sind vor Release nicht mehr vorhanden.

**Akzeptanzkriterien:**
- Ein ausführbarer Check-Command schlägt bei Verstößen fehl (Exit Code ≠ 0).
- Der Check ist in der Repo-Dokumentation kurz beschrieben.
