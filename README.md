# 🎬 Automatischer Folien-Generator für Gemeinde Konkordia

> Generiert automatisch professionelle PNG-Folien aus iCal-Kalenderdaten für Gottesdienst-Ansagen und Präsentationen.

## 🚀 Features

- ✅ **Vollautomatisch**: Täglich automatische Generierung via GitHub Actions
- 🎨 **Gemeinde-Design**: Roter Banner, weißer Footer, QR-Code integration
- 📅 **iCal-Integration**: Liest Termine direkt aus den iCal-Kalendern
- 🖼️ **Full HD**: 1920×1080px PNG-Folien für ProPresenter/Beamer
- 📺 **Web-Viewer**: Interaktive Präsentation mit Auto-Rotation
- ⏱️ **Smart-Timing**: 10s für Termine, 6s für Info-Folien
- 🔄 **3:1 Rhythmus**: Automatische Einstreuung von Info-Folien

## 📁 Projektstruktur

```
News---Aktuelles/
├── .github/
│   └── workflows/
│       ├── fetch_calendar.yml       # Aktualisiert iCal alle 2h
│       └── generate-slides.yml      # Generiert Folien täglich um 06:00
├── src/
│   ├── generator.py                 # Python-Script für PNG-Generierung
│   └── requirements.txt             # Python-Dependencies
├── slides/                          # Generierte PNG-Folien + Info-Folien
│   ├── slide_00_titel.png          # (automatisch generiert)
│   ├── slide_01_*.png              # (automatisch generiert)
│   ├── slide1.jpg                  # Statische Info-Folie 1
│   ├── slide2.jpg                  # Statische Info-Folie 2
│   └── ...
├── viewer/
│   └── index.html                  # Web-Präsentation
├── *.ics                           # iCal-Kalender (automatisch aktualisiert)
└── README.md
```

## 🎯 Wie es funktioniert

### 1. iCal-Daten werden aktualisiert
- GitHub Action `fetch_calendar.yml` läuft alle 2 Stunden
- Lädt aktuelle Kalenderdaten von ChurchTools
- Speichert in: `gottesdienst.ics`, `kinder.ics`, `senioren.ics`, `jugend.ics`, `sonstige.ics`

### 2. Folien werden generiert
- GitHub Action `generate-slides.yml` läuft täglich um 06:00 Uhr
- Python-Script parsed alle iCal-Dateien
- Filtert Events der nächsten 14 Tage
- Generiert PNG-Folien im Gemeinde-Design
- Integriert Info-Folien im 3:1 Rhythmus

### 3. Web-Viewer wird deployed
- Folien werden automatisch auf GitHub Pages deployed
- URL: `https://jcdappen.github.io/News---Aktuelles/`
- Auto-Rotation, Tastatur-Navigation, Vollbild-Modus

## 🛠️ Lokale Installation & Entwicklung

### Voraussetzungen
- Python 3.11 oder höher
- pip (Python Package Manager)
- Git

### Setup

```bash
# 1. Repository klonen
git clone https://github.com/jcdappen/News---Aktuelles.git
cd News---Aktuelles

# 2. Python-Dependencies installieren
pip install -r src/requirements.txt

# 3. Script ausführen
python src/generator.py

# 4. Viewer öffnen (lokal)
open viewer/index.html
# oder
python -m http.server 8000
# dann: http://localhost:8000/viewer/
```

## 🎨 Design-Spezifikationen

### Farben
- **Gemeinde-Rot**: `#bb2232` (RGB: 187, 34, 50)
- **Weiß**: `#ffffff`
- **Hintergrau**: `#f5f5f5`
- **Text-Grau**: `#333333`

### Layout (1920×1080px)
```
┌─────────────────────────────────┐
│   [Dunkler Hintergrund]         │
│                                  │
│  ┌──────────────────────────┐  │
│  │  VERANSTALTUNG (Rot)    │  │ ← Roter Banner (100-120px)
│  └──────────────────────────┘  │
│                                  │
│  Beschreibung (max 4 Zeilen)   │ ← Weiß, 42px
│                                  │
│  ┌──────────────────────────┐  │
│  │ Mittwoch                 │  │ ← Weißer Footer (280px)
│  │ 12. November 2025        │  │   + QR-Code (150×150px)
│  │ 🕐 19:00 - 21:00 Uhr    │  │
│  │ 📍 Adresse...      [QR] │  │
│  └──────────────────────────┘  │
└─────────────────────────────────┘
```

### Assets
- **Hintergrundbild**: [hintergrund_Termine.jpg](https://storage2.snappages.site/S74RKH/assets/files/hintergrund_Termine.jpg)
- **QR-Code**: [QR_news.png](https://storage2.snappages.site/S74RKH/assets/files/QR_news.png)

## ⚙️ Konfiguration

### Python-Script anpassen

Bearbeite `src/generator.py`:

```python
class Config:
    # Zeitfilter anpassen
    DAYS_AHEAD = 14  # Termine der nächsten X Tage

    # Timing anpassen
    TERMIN_DURATION = 10000   # 10 Sekunden
    INFO_DURATION = 6000      # 6 Sekunden

    # Farben anpassen
    GEMEINDE_ROT = (187, 34, 50)
```

### GitHub Action anpassen

Bearbeite `.github/workflows/generate-slides.yml`:

```yaml
schedule:
  - cron: '0 6 * * *'  # Täglich um 06:00 Uhr
```

**Cron-Beispiele:**
- `0 6 * * *` - Jeden Tag um 06:00 Uhr
- `0 */4 * * *` - Alle 4 Stunden
- `0 6 * * 0` - Jeden Sonntag um 06:00 Uhr

## 📝 Info-Folien hinzufügen

1. Erstelle deine Info-Folie als **JPG oder PNG** (1920×1080px)
2. Benenne sie: `slideX.jpg` (z.B. `slide7.jpg`)
3. Lege sie in den `slides/` Ordner
4. Commit & Push:

```bash
git add slides/slide7.jpg
git commit -m "Neue Info-Folie hinzugefügt"
git push
```

5. Beim nächsten Workflow-Lauf wird sie automatisch integriert!

**Wichtig:**
- Info-Folien werden im 3:1 Rhythmus eingestreut
- Benenne sie `slide1.jpg`, `slide2.jpg`, etc.
- Format: JPG oder PNG
- Größe: 1920×1080px

## 🌐 Web-Viewer Bedienung

### Tastatur-Shortcuts
- **←** / **→** - Vorherige/Nächste Folie
- **Leertaste** - Pause/Play
- **F** - Vollbild ein/aus
- **ESC** - Vollbild beenden / Hilfe schließen
- **H** - Hilfe ein/aus
- **R** - Neu laden

### Auto-Rotation
- **Termine**: 10 Sekunden
- **Info-Folien**: 6 Sekunden
- **Progress Bar** zeigt Fortschritt

### Features
- ✅ Auto-Play mit konfigurierbarem Timing
- ✅ Manuelle Navigation
- ✅ Vollbild-Modus
- ✅ Fortschrittsanzeige
- ✅ Seitenzahl-Anzeige
- ✅ Auto-Hide Cursor/Controls

## 🔧 Troubleshooting

### Problem: Keine Folien werden generiert

**Lösung:**
1. Prüfe GitHub Action Logs: `Actions` → `Generate Slides`
2. Stelle sicher, dass iCal-Dateien existieren
3. Prüfe ob Events in den nächsten 14 Tagen vorhanden sind

```bash
# Lokales Testing
python src/generator.py
```

### Problem: Falsche Schriftarten

**Lösung:**
Das Script verwendet automatisch verfügbare System-Fonts:
- Linux: DejaVu Sans
- Windows: Arial
- macOS: Helvetica

Für Custom Fonts, bearbeite `AssetManager._load_fonts()` in `src/generator.py`.

### Problem: Assets nicht geladen

**Lösung:**
Falls Hintergrundbild oder QR-Code nicht laden:
- Script erstellt automatisch Fallback-Hintergrund
- Prüfe Internet-Verbindung beim GitHub Action Lauf
- URLs in `Config` class prüfen

### Problem: GitHub Action schlägt fehl

**Lösung:**
1. Prüfe Permissions in Repository Settings → Actions
2. Stelle sicher, dass GitHub Pages aktiviert ist
3. Branch sollte `main` sein oder angepasst im Workflow

```yaml
permissions:
  contents: write
  pages: write
  id-token: write
```

### Problem: Viewer zeigt keine Folien

**Lösung:**
1. Prüfe ob `slides/slide_*.png` Dateien existieren
2. GitHub Pages muss aktiviert sein
3. Warte ~2 Minuten nach Deployment
4. Cache leeren: Strg+Shift+R

## 📊 GitHub Actions Workflow

### `fetch_calendar.yml`
- **Trigger**: Alle 2 Stunden + manuell
- **Zweck**: iCal-Dateien aktualisieren
- **Output**: `*.ics` Dateien

### `generate-slides.yml`
- **Trigger**: Täglich 06:00 Uhr + bei iCal-Änderungen + manuell
- **Zweck**: PNG-Folien generieren & deployen
- **Output**: `slides/slide_*.png` + GitHub Pages

### Manuelles Auslösen

1. Gehe zu: `Actions` Tab im Repository
2. Wähle Workflow: `Generate Slides`
3. Klicke: `Run workflow`
4. Bestätige mit: `Run workflow`

## 🎓 Verwendung in ProPresenter

1. Öffne den Web-Viewer: `https://jcdappen.github.io/News---Aktuelles/`
2. Aktiviere Vollbild (F-Taste)
3. In ProPresenter:
   - `Media` → `New Web`
   - URL eingeben
   - Als Folie einfügen

**Alternative:**
- Lade PNG-Folien direkt herunter:
  - `slides/slide_00_titel.png`
  - `slides/slide_01_*.png`
  - etc.
- Importiere in ProPresenter als Bilder

## 📅 Event-Datenformat (iCal)

Das Script erwartet folgende iCal-Felder:

```ical
BEGIN:VEVENT
SUMMARY:Gottesdienst
DESCRIPTION:Gemeinsamer Gottesdienst mit Abendmahl
LOCATION:Konkordia, Eisenbahnstraße 31, 77815 Bühl
DTSTART:20251115T100000Z
DTEND:20251115T113000Z
END:VEVENT
```

**Mapping:**
- `SUMMARY` → Titel (roter Banner)
- `DESCRIPTION` → Beschreibung (mittig)
- `LOCATION` → Adresse (Footer)
- `DTSTART` / `DTEND` → Datum & Uhrzeit (Footer)

## 🚦 Status & Monitoring

### Prüfe Folien-Generierung
```bash
# Lokal
ls -lh slides/slide_*.png

# GitHub
https://github.com/jcdappen/News---Aktuelles/actions
```

### Prüfe Deployment
```bash
# Live URL
https://jcdappen.github.io/News---Aktuelles/

# GitHub Pages Status
Settings → Pages → Your site is live at...
```

## 🤝 Beitragen

Verbesserungen und Vorschläge sind willkommen!

1. Fork das Repository
2. Erstelle einen Feature-Branch: `git checkout -b feature/neue-funktion`
3. Commit deine Änderungen: `git commit -m "Neue Funktion"`
4. Push zum Branch: `git push origin feature/neue-funktion`
5. Erstelle einen Pull Request

## 📄 Lizenz

Dieses Projekt ist für die **Gemeinde in der Konkordia** erstellt.

## 🆘 Support

Bei Fragen oder Problemen:
- Erstelle ein Issue: [GitHub Issues](https://github.com/jcdappen/News---Aktuelles/issues)
- Kontaktiere: [Gemeinde Konkordia](https://gemeindekonkordia.de)

---

**Erstellt mit ❤️ für die Gemeinde in der Konkordia**

*Automatisiert, professionell, einfach zu bedienen.*
