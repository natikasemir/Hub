# 🍸 Getränkekarten-Generator

**PETER EDEL – Toolbox**

Browser-basierter Generator für Getränke- und Speisekarten in verschiedenen Formaten. Unterstützt zwei CI-Schemas (PETER EDEL / kbw.), Live-Vorschau, Inline-Artikelbearbeitung und Export als PDF oder JPEG.

---

## Funktionsübersicht

### Kartengestaltung

- **Zwei CI-Schemas:** PETER EDEL (PE) und kbw. — steuert gleichzeitig das visuelle Design und das angezeigte Sortiment
- **Sechs Ausgabeformate:** A4 Hoch, A4 Quer, A3 Hoch, A3 Quer, DIN Lang, 16:9 Screen (4K)
- **Hintergrundbilder:** Pro Format und Schema wird ein eigenes Hintergrundbild geladen (enthält Logo und Design-Elemente)
- **Eigene Überschrift:** Freitextfeld für den Kartentitel (leer = kein Titel)
- **Preise ein-/ausblenden:** Toggle „Mit Preisen / Ohne Preise"
- **Allergen-Hinweise:** Drei Stufen — Ohne, Allgemein, Detailliert
- **Kategorie-Filter:** Checkboxen zum Ein-/Ausblenden einzelner Kategorien

### Artikelverwaltung (rechte Sidebar)

- **Artikelliste:** Zeigt alle Artikel des aktiven Schemas, gruppiert nach Kategorien
- **Inline-Bearbeitung:** Klick auf einen Artikel öffnet einen Dialog zum Ändern von Name, Variante, Beschreibung, Preis, Allergene, Kategorie und Angebot
- **Neue Artikel hinzufügen:** „+"-Button erstellt einen neuen Artikel im aktiven Schema
- **Artikel löschen:** Im Bearbeitungsdialog mit Bestätigungsabfrage
- **Excel-Export:** Geänderte Artikeldaten als .xlsx exportieren

### Export

- **PDF:** Alle Druckformate werden als PDF exportiert (Scale 2 für hohe Auflösung)
- **JPEG:** Alle Formate als JPEG exportierbar
- **DIN Lang:** Wird automatisch 3× nebeneinander auf A4 Quer gedruckt, mit grauen Schnittlinien
- **16:9 Screen:** Wird als 4K-JPEG (3840×2160) exportiert statt als PDF

### Spaltenverteilung

Die Kategorien werden per **sequentiellem Befüllungsalgorithmus** auf die verfügbaren Spalten verteilt:

1. Spalte 1 wird von oben nach unten befüllt (in der vorgegebenen Kategoriereihenfolge)
2. Wenn eine Kategorie nicht mehr passt, wird in die nächste Spalte gewechselt
3. Kategorien, die in keine Spalte mehr passen, werden ausgeblendet und in einer Warnung benannt
4. Schriftgrößen werden nie verändert — Lesbarkeit bleibt konstant

### Overflow-Warnung

Wenn nicht alle Kategorien auf das gewählte Format passen, erscheint ein gelber Hinweis über der Vorschau mit den Namen der ausgeblendeten Kategorien.

---

## Dateistruktur

```
Hub/
├── config.json                          # Zentrale Konfiguration
├── tools/
│   └── kartengenerator.html             # Hauptdatei (Single-Page-App)
├── assets/
│   ├── styles/
│   │   ├── stylesheet.css               # Globales Stylesheet (Fonts, App-Shell)
│   │   └── menu-drinks.css              # Kartenspezifisches Stylesheet
│   ├── fonts/
│   │   ├── SourceSans3-VariableFont_wght.ttf
│   │   ├── URWDIN-*.ttf                 # URW DIN (alle Gewichte)
│   │   └── Azo_Sans_*.otf              # Azo Sans (alle Gewichte)
│   └── images/
│       └── backgrounds/
│           ├── Background_A4P--peter-edel.jpg
│           ├── Background_A4P--kbw.jpg
│           ├── Background_A4L--peter-edel.jpg
│           ├── Background_A4L--kbw.jpg
│           ├── Background_A3P--peter-edel.jpg
│           ├── Background_A3P--kbw.jpg
│           ├── Background_A3L--peter-edel.jpg
│           ├── Background_A3L--kbw.jpg
│           ├── Background_DL--peter-edel.jpg
│           ├── Background_DL--kbw.jpg
│           ├── Background_SCREEN--peter-edel.jpg
│           └── Background_SCREEN--kbw.jpg
```

---

## Abhängigkeiten

Alle externen Libraries werden über CDN geladen (kein Build-Prozess nötig):

| Library | Version | Zweck |
|---------|---------|-------|
| [SheetJS (xlsx)](https://sheetjs.com/) | 0.18.5 | Excel-Import und -Export (.xlsx) |
| [html2canvas](https://html2canvas.hertzen.com/) | 1.4.1 | DOM-zu-Canvas-Rendering für PDF/JPEG-Export |
| [jsPDF](https://github.com/parallax/jsPDF) | 2.5.1 | PDF-Erzeugung aus Canvas-Daten |

### Schriften

| Font | Verwendung | Format |
|------|-----------|--------|
| Source Sans 3 | Basis-Schrift (kbw.-Schema), Fließtext | .ttf (Variable Font) |
| URW DIN | Basis-Schrift (PE-Schema), Fließtext | .ttf |
| Azo Sans | Kategorie- und Kartentitel (nur PE-Schema), Gewicht: Uber Regular (950) | .otf |

---

## config.json

Die zentrale Konfigurationsdatei steuert mehrere Aspekte des Generators:

```json
{
  "tools": {
    "kartengenerator": {
      "version": "2026-03-26",
      "categoryOrder": [
        "Alkoholfreie Getränke",
        "Heißgetränke",
        "Bier",
        "Wein & Sekt",
        "Longdrinks",
        "Shots",
        "Specials",
        "Snacks"
      ]
    }
  },
  "contact": {
    "email": "beispiel@peteredel.de"
  }
}
```

| Key | Typ | Beschreibung |
|-----|-----|-------------|
| `tools.kartengenerator.version` | String | Kartenversion, wird im Header angezeigt |
| `tools.kartengenerator.categoryOrder` | Array | Reihenfolge der Kategorien auf der Karte. Kategorien, die nicht in der Liste stehen, werden vor „Snacks" eingefügt. |
| `contact.email` | String | E-Mail-Adresse für den Feedback-Button |

---

## Datenformat (Excel / CSV)

Der Generator akzeptiert `.xlsx`, `.xls` und `.csv`-Dateien mit folgenden Spalten:

| Spalte | Pflicht | Beschreibung |
|--------|---------|-------------|
| Kategorie | ✓ | z.B. „Alkoholfreie Getränke", „Bier", „Longdrinks" |
| Name | ✓ | Artikelname, z.B. „fritz-kola" |
| Variante | | Größenangabe, z.B. „0,33l" |
| Beschreibung | | Zusatzinfo, z.B. „Classic \| Naturell" |
| Preis | | Preis mit Währung, z.B. „4,00 €" |
| Allergene | | Kommagetrennte Kennzeichnungsnummern, z.B. „1,11" |
| Angebot | ✓ | `peter-edel` oder `kbw` — bestimmt, in welchem Schema der Artikel angezeigt wird |

### Eingebettete Standarddaten

Die aktuelle Artikelliste ist direkt im Code eingebettet (`getDefaultData()`). Beim Laden der Seite werden die Daten sofort gerendert — kein Upload nötig. Ein Datei-Upload überschreibt die Standarddaten für die aktuelle Session.

---

## Hintergrundbilder

Pro Format und Schema wird ein Hintergrundbild geladen, das Logo und Design-Elemente enthält. Die Bilder liegen als `.jpg` unter `assets/images/backgrounds/`.

### Empfohlene Auflösungen (Minimum)

| Format | Pixel (2×) |
|--------|-----------|
| A4 Hoch | 1190 × 1684 |
| A4 Quer | 1684 × 1190 |
| A3 Hoch | 1684 × 2382 |
| A3 Quer | 2382 × 1684 |
| DIN Lang | 594 × 1260 |
| Screen 4K | 3840 × 2160 |

Die Bilder werden als `<img>` im DOM gerendert (nicht als CSS-Background), damit html2canvas sie in voller Auflösung erfasst.

### Dateinamens-Konvention

```
Background_[FORMAT]--[SCHEMA].jpg
```

Beispiele: `Background_A4P--peter-edel.jpg`, `Background_DL--kbw.jpg`

---

## Allergen-Kennzeichnung

Drei Stufen, wählbar in der Sidebar:

- **Ohne:** Kein Hinweis
- **Allgemein:** „Bei Fragen zu Inhaltsstoffen und Allergenen steht Ihnen unser Team gern zur Seite."
- **Detailliert:** Vollständige Auflistung aller Kennzeichnungsnummern (1-Farbstoff, 2-Aroma, 16-Alkohol, etc.)

Einzelne Artikel können in der Spalte „Allergene" kommagetrennte Kennzeichnungsnummern enthalten (z.B. `1,11`), die als Hochzahlen hinter dem Artikelnamen dargestellt werden.

---

## Typografie nach Schema

| Element | PE-Schema | kbw.-Schema |
|---------|-----------|-------------|
| Fließtext | URW DIN | Source Sans 3 |
| Kategorietitel | Azo Sans Uber Regular (950) | Source Sans 3 Bold (800) |
| Kartentitel | Azo Sans Uber Regular (950) | Source Sans 3 Bold (800) |
| CI-Farbe | #173557 (Dunkelblau) | #8c084d (Magenta) |

---

## Lokale Entwicklung

Der Generator funktioniert direkt im Browser — kein Build-Prozess nötig. Für die volle Funktionalität (config.json, Hintergrundbilder, Fonts) empfiehlt sich ein lokaler Webserver:

```bash
cd /pfad/zum/Hub
npx serve .
```

Dann im Browser `http://localhost:3000/tools/kartengenerator.html` aufrufen.

> **Hinweis:** Ohne Webserver (direktes Öffnen via `file://`) funktioniert der Generator grundsätzlich, aber `config.json` kann nicht geladen werden. Die Standardwerte greifen dann automatisch.

---

## Bekannte Einschränkungen

- **SheetJS Community Edition:** Unterstützt keine Zell-Formatierung (Farben, fette Header) beim Excel-Export. Die Spaltenbreiten, Freeze Pane und AutoFilter funktionieren.
- **html2canvas:** SVG-Logos werden nicht zuverlässig gerendert — daher die Lösung mit Hintergrundbildern als JPG.
- **Schriftgrößen:** Sind pro Format fest definiert und werden nicht dynamisch angepasst. Bei zu viel Inhalt werden Kategorien ausgeblendet statt die Schrift zu verkleinern.

---

*Erstellt im Rahmen des PETER EDEL-Toolkit · Website-Management*