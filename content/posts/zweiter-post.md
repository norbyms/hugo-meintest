title: Zweiter Post
date: 2026-01-11T22:41:00.000+01:00
draft: false
tags:
  - TestDecapCMS
body: >
  Sehr gut, die Fehlermeldung ist **aussagekräftig**.

  Und ja: **das ist kein OAuth-Problem und kein YAML-Problem**.

  Es ist **HTML-Timing**.


  ---


  ## Ursache (klar benannt)


  Der entscheidende Fehler ist:


  ```

  Uncaught TypeError: Cannot read properties of null (reading 'appendChild')

  ```


  Decap CMS versucht beim Laden, sich **an `document.body` anzuhängen**.


  In deinem `index.html` passiert aber Folgendes:


  * Das `<script>` wird **im `<head>`** geladen

  * Zu diesem Zeitpunkt existiert `document.body` **noch nicht**

  * Ergebnis: `appendChild(null)` → **Abbruch → weiße Seite**


  ➡️ **Das Script lädt, aber zu früh.**


  ---


  ## Korrekte Lösung (minimal, sauber)


  ### Variante A (empfohlen): Script ans Ende des `<body>`


  Ändere **`static/admin/index.html`** zu:


  ```html

  <!doctype html>

  <html lang="de">

  <head>
    <meta charset="utf-8" />
    <title>Content Manager</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
  </head>

  <body>

    <script src="https://unpkg.com/decap-cms@latest/dist/decap-cms.js"></script>

  </body>

  </html>

  ```


  Das ist **die Standard-Variante**, die Decap erwartet.


  ---


  ### Variante B (alternativ): `defer` (auch ok)


  ```html

  <script defer src="https://unpkg.com/decap-cms@latest/dist/decap-cms.js"></script>

  ```


  Aber: **Variante A ist narrensicher**.


  ---


  ## Nebenbefund (nicht blocker, aber korrekt)


  ```yaml

  label: "Beitr├ñge"

  ```


  Das ist ein **Encoding-Artefakt** (BOM / falsches Encoding).


  ➡️ Öffne `config.yml` in:


  * VS Code

  * Notepad++

  * **UTF-8 ohne BOM** speichern


  Dann steht dort korrekt:


  ```yaml

  label: "Beiträge"

  ```


  Das hat **nicht** den White-Screen verursacht, sollte aber korrigiert werden.


  ---


  ## Vorgehen jetzt (bitte exakt so)


  1. `index.html` Script **in den Body verschieben**

  2. Datei speichern

  3. `hugo server` ggf. neu starten

  4. Browser **hart neu laden** (`Strg + F5`)

  5. `/admin/` erneut öffnen


  ---


  ## Erwartetes Ergebnis


  * CMS-UI erscheint

  * GitHub-Login startet

  * Keine `appendChild`-Fehler mehr


  ---


  ## Kurzes Fazit


  * Dein HTML war **fast richtig**

  * Decap ist **extrem empfindlich** bei Script-Timing

  * Das ist ein **klassischer Anfänger-Stolperstein**, kein Denkfehler


  Mach die Änderung und sag dann nur:


  * ✅ „CMS lädt“

  * ❌ oder neue konkrete Fehlermeldung


  Dann gehen wir **direkt zum ersten Artikel über das CMS**.
