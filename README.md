# DOKUMENTATION
**GÜNTHER** (**G**esamt**ü**bersicht **N**ützlicher **T**ools für **H**ospitality, **E**vents und **R**estaurantbetrieb) ist eine interne Web-Toolbox für den Veranstaltungs- und Gastronomiebetrieb des PETER EDEL und kbw. Sie bündelt digitale Hilfsmittel, die im Alltag wiederkehrend gebraucht werden — schnell zugänglich, ohne Installation, direkt im Browser.

## Tools

1. 🖨️ **Schild-Generator**
   
   *In Entwicklung*
   
   Erstellt Beschilderungen und Plakate in den Formaten A6 bis A3, wahlweise Hoch- oder Querformat. Unterstützt beide CI-Schemas (PE und kbw.) und erlaubt Layouts mit oder ohne Fließtext. Ausgabe direkt als Druck oder PDF.

3. 🍸 **ZAPFHAHN — Getränkekarten-Generator (in Entwicklung)**
   
   *In Entwicklung*
   
   Generiert Bistro- und Bar-Getränkekarten auf Basis einer CSV- oder Excel-Datei. Unterstützt sechs Kartenvarianten (Bistro Normal, Bistro Spezial, Bar Normal, Bar Spezial, 16:9 Screen sowie weitere), alle gängigen Druckformate (A4, A3, DIN LANG) sowie ein 4K-Screenformat für Bildschirmpräsentation. Ausgabe als Druck, PDF oder JPEG.

   **Features:**
   - CSV- und Excel-Upload mit automatischer Trennzeichen-Erkennung
   - Allergen-Kennzeichnung als hochgestellte Superscripts
   - Masonry-Layout für optimale Flächennutzung
   - Zwei CI-Schemas (PE blau / kbw. magenta)
   - Vorlage als Download verfügbar

3. 🧹 **Reinigungsplan**
  
   *In Entwicklung*
   
   Automatisch generierter Reinigungsplan auf Basis des JSON-Exports aus dem Veranstaltungsmanagementsystem. Läuft auf dem internen Server.

## Technisches
GÜNTHER ist eine reine Frontend-Anwendung — kein Backend, keine Datenbank, keine Abhängigkeiten außer einem modernen Browser. Alle Tools laufen lokal im Browser des Nutzers.



## CI-Schemata

**Schema 1: PE**
- Hauptfarbe: #173557
- Akzentfarbe: #EB5E50
- Schriftart: URW DIN

**Schema 2: kbw.**
- Hauptfarbe: #8c084d
- Akzentfarbe: #EB5E50
- Schriftart: Source Sans 3

## Sonstiges

**Weitere Namen als Alternative für GÜNTHER**

- ZAPFHAHN (Zentrale Anwendung für Planung, Formulare, Hinweisschilder, Aufgaben, Hilfsmittel und Nützliches)
- HENDL (Hilfssystem für Events, Namensschilder, Dokumente und Logistik)
- BREZN (Browserbasiertes Repository für Events, Zeitpläne und Nützliches)
