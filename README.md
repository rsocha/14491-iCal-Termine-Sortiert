# iCal-Termine Controller - HSL 2.0 für HomeServer 4.13

## Übersicht

Es gibt zwei Bausteine:

| Baustein | ID | Beschreibung |
|----------|-----|--------------|
| **iCal-Termine** | 14490 | Suche nach spezifischen Terminen mit Vorwarnzeit |
| **iCal-Termine-Sortiert** | 14491 | Die nächsten 5 Termine sortiert ausgeben |

---

## Features

| Feature | 14490 | 14491 |
|---------|-------|-------|
| iCal/ICS-Datei einlesen | ✅ | ✅ |
| Suchbegriff-Filterung | ✅ (5 Suchbegriffe) | ❌ |
| Vorwarnzeit (VWZ) | ✅ (individuell) | ❌ |
| Sortierte Ausgabe | ❌ | ✅ (Top 5) |
| Unicode/Umlaute | ✅ | ✅ |
| Debug-Panel | ✅ | ✅ |

---

## Installation


## iCal-Quellen

### Unterstützte Formate

| Quelle | Beispiel |
|--------|----------|
| Lokale Datei | `http://192.168.1.100/kalender/muell.ics` |
| Webserver | `https://example.com/calendar.ics` |
| Abfallkalender | Viele Kommunen bieten ICS-Exporte an |
| Google Calendar | Private ICS-URL aus Kalendereinstellungen |

### Beispiel-URLs

```
# Lokaler Webserver (EDOMI, Homeserver, NAS)
http://192.168.0.99/edomi/muellkalender.ics

# Öffentlicher Feiertagskalender (Test)
https://www.calendarlabs.com/ical-calendar/ics/46/Germany_Holidays.ics
```

---

## Baustein 14490: iCal-Termine (mit Suchbegriffen)

### Eingänge

| # | Eingang | Typ | Beschreibung |
|---|---------|-----|--------------|
| 1 | URL | String | Pfad zur ICS-Datei |
| 2 | Trigger | Number | Bei Wert ≠ 0 wird die Datei abgerufen |
| 3 | ICAL 1 | String | Suchbegriff 1 (z.B. "Restmüll") |
| 4 | ICAL 2 | String | Suchbegriff 2 (z.B. "Gelber Sack") |
| 5 | ICAL 3 | String | Suchbegriff 3 (z.B. "Biomüll") |
| 6 | ICAL 4 | String | Suchbegriff 4 |
| 7 | ICAL 5 | String | Suchbegriff 5 |
| 8 | VWZ 1 | Number | Vorwarnzeit in Tagen für ICAL 1 |
| 9 | VWZ 2 | Number | Vorwarnzeit in Tagen für ICAL 2 |
| 10 | VWZ 3 | Number | Vorwarnzeit in Tagen für ICAL 3 |
| 11 | VWZ 4 | Number | Vorwarnzeit in Tagen für ICAL 4 |
| 12 | VWZ 5 | Number | Vorwarnzeit in Tagen für ICAL 5 |

### Ausgänge

| # | Ausgang | Typ | Beschreibung |
|---|---------|-----|--------------|
| 1 | Summe Warnungen | Number | Anzahl aktiver Vorwarnungen |
| 2 | Text 1 | String | Gefundener Termin-Text für ICAL 1 |
| 3 | Vorwarnung 1 | Number | 1 = Vorwarnung aktiv für ICAL 1 |
| 4 | Datum 1 | String | Nächster Termin (DD.MM.YYYY) |
| 5 | Wochentag 1 | String | Wochentag des Termins |
| 6 | Tage 1 | Number | Tage bis zum Termin |
| 7-26 | ... | ... | Wiederholt sich für ICAL 2-5 |

### Matching-Algorithmus

- **Teilstring-Suche**: "Müll" findet "Restmüll", "Biomüll", "Müllabfuhr"
- **Case-Insensitive**: "müll" = "Müll" = "MÜLL"
- **Unicode-sicher**: Umlaute (ä, ö, ü, ß) werden korrekt verarbeitet

---

## Baustein 14491: iCal-Termine-Sortiert

### Eingänge

| # | Eingang | Typ | Beschreibung |
|---|---------|-----|--------------|
| 1 | URL | String | Pfad zur ICS-Datei |
| 2 | Trigger | Number | Bei Wert ≠ 0 wird die Datei abgerufen |

### Ausgänge

| # | Ausgang | Typ | Beschreibung |
|---|---------|-----|--------------|
| 1 | Termin 1 Text | String | Nächster Termin (Summary) |
| 2 | Termin 1 Datum | String | Datum (DD.MM.YYYY) |
| 3 | Termin 1 Wochentag | String | Wochentag |
| 4 | Termin 1 Tage | Number | Tage bis zum Termin |
| 5-16 | ... | ... | Wiederholt sich für Termin 2-5 |

### Sortierung

- Termine werden nach Datum aufsteigend sortiert
- Nur zukünftige Termine (ab gestern) werden angezeigt
- Bei weniger als 5 Terminen bleiben Ausgänge leer

---

## Konfigurationsbeispiele

### Müllabfuhr-Kalender

```
URL        = "http://192.168.0.xx/xxx.ics"
Trigger    = 1  (einmal täglich triggern)
ICAL 1     = "Restmüll"
ICAL 2     = "Gelber Sack"
ICAL 3     = "Biomüll"
ICAL 4     = "Papier"
ICAL 5     = ""
VWZ 1-4    = 1  (1 Tag vorher warnen)
```

### Feiertagskalender

```
URL        = "http://192.168.0.xx/xxx.ics"
ICAL 1     = "Christmas"
ICAL 2     = "Easter"
VWZ 1-2    = 7  (1 Woche vorher warnen)
```

---

## Vorwarnzeit (VWZ)

Die Vorwarnzeit definiert, wann der Ausgang "Vorwarnung" auf 1 gesetzt wird.

| VWZ | Bedeutung |
|-----|-----------|
| 0 | Keine Vorwarnung |
| 1 | 1 Tag vorher (am Vortag) |
| 2 | 2 Tage vorher |
| 7 | 1 Woche vorher |

### Beispiel: VWZ = 1

```
Heute:     Montag, 13.01.2026
Termin:    Dienstag, 14.01.2026 (Restmüll)
Vorwarnung = 1  ✓ (1 Tag vorher)
```

---

## KNX-Integration Beispiele

### Mülltonnen-Erinnerung

```
Vorwarnung 1 = 1  → "Restmüll morgen rausstellen!"
Vorwarnung 2 = 1  → "Gelber Sack morgen rausstellen!"
Summe > 0        → Allgemeine Müll-Warnung
```

### Anzeige auf KNX-Display

```
Text 1 + Datum 1      → "Restmüll - 14.01.2026"
Wochentag 1 + Tage 1  → "Dienstag (in 1 Tag)"
```

### Zeitgesteuerte Abfrage

```
Zeitschaltuhr (06:00) → Trigger = 1  (täglich um 6 Uhr aktualisieren)
```

---

## Debug-Seite

Erreichbar unter: `http://<HomeServer-IP>/hslist

### Angezeigte Werte (14490)

| Feld | Beschreibung |
|------|--------------|
| Baustein ID | 14490 |
| Version | Aktuelle Modulversion |
| Status | "Initialisiert" / "Daten abgerufen" |
| URL | Konfigurierte iCal-URL |
| Events gefunden | Anzahl der VEVENT-Einträge |
| Zukünftige Events | Gefilterte Termine ab heute |
| Last Update | Zeitpunkt der letzten Aktualisierung |
| Last Error | Letzte Fehlermeldung |

---

## Troubleshooting

### Baustein erscheint nicht
- Datei im richtigen Ordner?
- Experten komplett neu starten

### Keine Termine gefunden
- URL im Browser testen
- ICS-Datei enthält VEVENT-Einträge?
- Termine liegen in der Zukunft?

### Suchbegriff findet nichts
- Groß-/Kleinschreibung ist egal
- Exakten Text aus der ICS-Datei prüfen
- Debug-Seite: "Events gefunden" prüfen

### Unicode/Umlaut-Fehler
- Module verwenden iso-8859-15 Encoding
- Bei Problemen: Suchbegriff ohne Umlaute testen
- Alternativ: Umlaut-Varianten (ae statt ä)

### Timeout beim Abruf
- URL erreichbar? (ping, Browser-Test)
- Firewall blockiert Verbindung?
- Lokale URL statt Internet-URL verwenden

### "codec can't encode/decode"
- Python 2.7 Unicode-Problem
- Module sind dafür vorbereitet
- Ggf. Testskript standalone ausführen



## Technische Details

### Abhängigkeiten

- Python 2.7 (HLS 2.0 Umgebung)
- `icalendar` Bibliothek (in HLS enthalten)
- `datetime`, `re`, `socket`, `urllib2`

### Encoding

- **Eingabe**: UTF-8 (iCal-Standard)
- **Verarbeitung**: Unicode intern
- **Ausgabe**: iso-8859-15 (Gira Homeserver Kompatibilität)

### Timeout

- Standard: 15 Sekunden für URL-Abruf
- Konfigurierbar im Quellcode

### Sortierung (14491)

- Vergangene Termine werden gefiltert (ab gestern)
- Termine werden chronologisch sortiert
- Nur die nächsten 5 Termine werden ausgegeben

---

## Changelog

### Version 1.2 (aktuell)
- **Unicode-Stabilität** verbessert
- Python 2/3 Kompatibilität im Testskript
- Robuste Encoding-Behandlung (UTF-8 → iso-8859-15)
- Standardisiertes Debug-Panel

### Version 1.1
- Vorwarnzeit (VWZ) pro Suchbegriff
- Teilstring-Suche (case-insensitive)
- Sortierte Ausgabe (14491)

### Version 1.0
- Erste Version (14490)
- Basis iCal-Parsing
- 5 Suchbegriffe

---

## Lizenz

MIT License

Copyright 2023-2026

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
