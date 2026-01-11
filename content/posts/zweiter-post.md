title: Dritter Post
date: 2026-01-11T22:42:00.000+01:00
draft: false
tags:
  - Seite 3 TestDecapCMS
body: |-
  
  noch eine Seite 
  ---

  ## Seite 3

  Hier Text 

  ```
  Codeblock
  ```

  Decap CMS versucht beim Laden, sich **an `document.body` anzuhängen**.

  In deinem `index.html` passiert aber Folgendes:

  * Das `<script>` wird **im `<head>`** geladen
  * Zu diesem Zeitpunkt existiert `document.body` **noch nicht**
  * Ergebnis: `appendChild(null)` → **Abbruch → weiße Seite**

  ➡️ **Das Script lädt, aber zu früh.**

  ---
