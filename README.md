# 14491 iCal-Termine-Sortiert

## Beschreibung 

Der Baustein liest ein ics File von einer url aus und gibt diese Aufbereitet aus. z.B. für Muellabfuhr. Der naechste faellige Termin steht immer als erstes.

## Eingänge

| Nr. | Name              | Initialisierung   | Beschreibung                                                                                          |
|-----|-------------------|-------------------|-------------------------------------------------------------------------------------------------------|
| 1   | url               | 0                 | Hier den Pfad zu dem ics File                                                                         |
| 2   | Trigger           | 0                 | Triggereingang für die Auswertung                                                                     |
| 3   | Name              | 0                 | Hier muss der Name stehen, der im ics File unter "Summery" steht. Es duerfen keine Umlaute vorkommen! |
| 4   | Vwz               | 0                 | Eingabe in Tagen ab wann die "Vorwarnung" für den nächsten Termin an den Ausgängen erfolgen soll      |    

## Ausgänge

| Nr. | Name              | Initialisierung | Beschreibung                                            |
|-----|-------------------|-----------------|---------------------------------------------------------|
| 1   | Summe Warnungen   | 0               | Ausgabe aller Warnungen einen Tag vor fälligen Datum.   |
| 2   | Text Tonne 1      | ''              | Text der Tonne.                                         |
| 3   | Vorwarnung 1      | 0               | Ausgabe für Vorwarnung 1 vor Datum von A4               |
| 4   | nächster Termin 1 | ''              | nächster fälliger Abholtermin.                          |
| 5   | Wochentag 1       | ''              | Wochentag zu Datum von A4                               |
| 6   | anzahl Tage       | 0               | Die Anzahl der Tage von heute bis zum fälligen Datum A4 |
| 7   | fff               |                 | Wiederholt sich wie A2 - A6                             |



## Beispielwerte

| Eingang | Ausgang |
| --- | --- |
| - | - |


## Other

- Neuberechnug beim Start: Nein
- Baustein ist remanent: nein
- Interne Bezeichnung: 14491
- Kategorie: Datenaustausch

### Change Log


   - v.03
     - div. Anpassungen wenn Datum mit Uhrzeit
   - v.02
     - Codierung Umlaute
   - v0.1
     - Initial

    


## Licence

Copyright 2023

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
