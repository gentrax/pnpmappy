# SR//GRID — Interaktives Karten-Tool für Pen-&-Paper-Runden

Ein eigenständiges, browserbasiertes Karten-Tool für Spielleiter:innen (GM). Lade eine beliebige Karte als Bild, setze nummerierte **Marker**, zeichne **Gebiete & Grenzen**, ordne alles über eine **Legende** mit Kategorien, miss **Distanzen** auf einem pro Karte geeichten Maßstab – und gib deiner Crew einen abgespeckten **Runner-Modus**.

Ursprünglich für eine Shadowrun-Hamburg-Kampagne gebaut, funktioniert das Tool mit jeder Karte (z. B. auch Fantasy-/D&D-Karten dank umschaltbarem Look).

> **Eine Datei, keine Installation.** Das gesamte Tool steckt in einer einzigen `.html`-Datei – einfach im Browser öffnen oder über GitHub Pages hosten.

---

## Features

- 🗺️ **Karte laden** per Datei-Upload oder Drag-&-Drop (JPG/PNG)
- 📍 **Marker** setzen, benennen, kategorisieren, mit Notiz versehen, verschieben & löschen
- 🟧 **Gebiete** (gefüllte Flächen) und **Grenzen / Linien** (gestrichelt) einzeichnen
- 🏷️ **Legende** mit frei definierbaren Kategorien (Farbe + Ein-/Ausblenden je Kategorie)
- 📏 **Distanzmessung** – einmalig am Maßstabsbalken eichen, dann über die ganze Karte messen
- 🎨 **Themes** umschaltbar: *Sixth World* (Cyberpunk) & *Fantasy* (Pergament/Gold)
- 👥 **GM- und Runner-Modus** – Spieler:innen sehen eine reduzierte, nicht editierbare Ansicht
- 💾 **Import/Export** als JSON (inkl. eingebetteter Karte) – jede Karte ist ein eigenständiges Savefile
- 🖼️ **Bild-Export (JPG)** der Karte inkl. aller sichtbaren Marker & Zonen – ideal als Handout
- ✏️ Editierbarer **Titel & Untertitel**
- 🌐 **GitHub-Pages-tauglich**: lädt beim Start automatisch eine passende JSON aus demselben Ordner

---

## Schnellstart

### Lokal
1. `index.html` herunterladen.
2. Doppelklick → öffnet sich im Browser.
3. **KARTE** klicken und ein Kartenbild laden – oder eine fertige `*.json` über **IMPORT** öffnen.

> Tipp: Für eine reine Spieler-Ansicht den Link teilen und in den **Runner**-Modus wechseln lassen – Bearbeitungs-Buttons sind dort ausgeblendet.

---

## Bedienung

### Modi
| Modus | Zweck |
|-------|-------|
| **GM** | Voller Zugriff: Marker/Zonen anlegen & bearbeiten, Kategorien, Karte laden, Maßstab eichen, Titel ändern |
| **Runner** | Nur ansehen, zoomen, Kategorien ein-/ausblenden und **messen** |

### Werkzeuge (Werkzeugleiste oben)
- **MARKER** – auf die Karte klicken, um einen Punkt zu setzen
- **GEBIET** – Eckpunkte klicken, Doppelklick/Enter schließt die Fläche
- **GRENZE** – Punkte klicken, Doppelklick/Enter beendet die Linie
- **MESSEN** – Distanz messen (siehe unten)
- **KARTE / IMPORT / EXPORT / BILD** – laden, speichern, als Bild exportieren
- **Design** – Theme wechseln

### Distanz messen & Maßstab eichen
1. **MESSEN** aktivieren → unten links erscheint das Distanz-Panel.
2. *(einmal pro Karte, nur GM)* **Maßstab eichen** klicken und **Anfang + Ende des Maßstabsbalkens** auf der Karte anklicken; dann reale Länge + Einheit (z. B. `5` / `km`) eintragen.
3. Danach beliebige Strecken klicken – Teil- und Gesamtdistanz (Σ) werden in der echten Einheit angezeigt.

Der geeichte Maßstab wird in der JSON gespeichert und gilt für die ganze Karte – unabhängig von Zoom & Verschiebung.

### Tastenkürzel
| Taste | Aktion |
|-------|--------|
| `A` | Marker-Werkzeug |
| `G` | Gebiet zeichnen |
| `L` | Grenze / Linie zeichnen |
| `M` | Messen |
| `Enter` / Doppelklick | Zeichnung/Messung abschließen |
| `Backspace` | letzten Punkt entfernen |
| `Esc` | Werkzeug abbrechen / Messung zurücksetzen |

---

## JSON-Format (Version 3)

Jede Karte wird als eine JSON-Datei gespeichert. Die Karte selbst ist als Daten-URL eingebettet, sodass die Datei eigenständig ist.

```jsonc
{
  "version": 3,
  "title": "Wildost",
  "subtitle": "REISEZIEL: WILDOST · STAND OKT 2080",
  "theme": "cyber",                // "cyber" | "fantasy"
  "scale": {                        // optional, null wenn nicht geeicht
    "unit": "km",
    "ref": 5,                       // eingegebene reale Länge
    "perPx": 0.0123,                // reale Einheiten je Bildpixel
    "a": { "x": 70.1, "y": 4.2 },   // Eich-Punkte (Prozent der Bildgröße)
    "b": { "x": 78.9, "y": 4.3 }
  },
  "map": "data:image/jpeg;base64,…",// eingebettete Karte
  "imgW": 3658,
  "imgH": 1323,
  "cats": [
    { "id": "ausgehen", "label": "Ausgehen", "color": "#ff2d6f" }
  ],
  "pins": [
    {
      "id": "w1",
      "x": 80.5,                    // Position in % der Bildbreite
      "y": 6.3,                     // Position in % der Bildhöhe
      "cat": "ausgehen",
      "name": "1 Doppel:U",
      "desc": "Kurze Notiz / Info zum Ort (max. 2 Sätze)."
    }
  ],
  "zones": [
    {
      "id": "z1",
      "closed": true,               // true = Fläche, false = Linie/Grenze
      "pts": [ { "x": 10, "y": 20 }, { "x": 30, "y": 25 } ],
      "cat": "sonst",
      "name": "Sperrzone",
      "desc": ""
    }
  ]
}
```

**Koordinaten** sind als Prozent der Bildgröße gespeichert (auflösungsunabhängig). Damit bleiben Marker an ihrer Stelle, egal wie die Karte angezeigt wird.

---

## Technik

- Reines HTML/CSS/JavaScript, **keine Build-Tools, keine Abhängigkeiten**
- Schriften via Google Fonts (Chakra Petch, Share Tech Mono, Cinzel, EB Garamond)
- Läuft in jedem aktuellen Browser; alle Daten bleiben lokal im Browser bzw. in deinen JSON-Dateien
