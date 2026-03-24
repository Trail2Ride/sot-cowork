# Ausgangslage

Das Standardmodul für Byron/BIS wurde beim Landtag Sachen eingeführt und zum Teil an die Anforderungen des Landtag Sachsen angepasst.

Damit das Modul Vertragsmanagement produktiv eingesetzt werden kann, sind jedoch weitere Anpassungen erforderlich. Dieser Anpassungsbedarf wurde im Rahmen eines Workshops und weiteren internen Abklärungen erhoben.

Dieser Anpassungsbedarf ist in die folgenden Dokumente eingeflossen, welche die Basis dieses Pflichtenhefts bilden:

-   `CM_Funktionsliste_fuer_neues_Angebot V4.xlsx`
-   `Vorschlag Vertragslaufzeit.xlsx`

# Zu klärende Punkte
## Vertragsversion

==Int. Frage: Gibt es immer eine Vertragsversion? Wie wird die Nummer generiert (Pkt. 45)==

# Anpassungsbedarf
Nachfolgend der Anpassungsbedarf gemäss den in der Ausgangslage geschilderten Dokumente

## Firma (Vertragspartner)

- Bearbeitbarkeit der Firmen resp. der Assoziation zur Person müssen auf Benutzer der Benutzergruppe `CAFM_CM_KeyUsers` beschränkt werden, alle anderen Anwender dürfen die Firma nur Read-Only sehen.
- Funktion ergänzen, dass von der Firma aus eine neue Person hinzugefügt werden kann.
- Funktion ergänzen, dass von der Firma aus aus einer Lookup-Combobox eine Person mit der Firma verknüpft werden kann.
- Datenmodell erweitern
	-   V1: Funktion für die Person
	-   V2: Funktion für Zwischenobjekt `PersonToFirma`

==Frage: Muss die Funktion der Person je Firma individuell festgelegt werden können, oder ist die Funktion der Person generell. Ersteres wäre deutlich aufwändiger in der Umsetzung.==

## Vertragseigenschaften

### Vertragsart/-Typ
==Int. Frage: Kann die Differenzierung gestrichen werden?==
> Gemäss Besprechung mit G. Fritsche soll dies mal so probiert werden
#### Vertragsart/-Typ anpassen (V1)

**Vertragstyp** *NEU*

- beinhaltet neu die Enum-Werte der Vertragsart (unbefristet, befristet, ....)
- Soll an die Vertragsversion
- 
**Vertragstyp** *BISHER*
- fällt weg

#### Vertragsart/-Typ fällt weg (V2 bevorzugt)

- Vertragsarten / Typen fallen weg

### Eigenschaft Ausschreibung

- Eigenschaft "EU-Ausschreibung" umbenennen in "Ausschreibung".
- Eigenschaft «Ausschreibung» in Ansicht «Verträge» ergänzen.

### Funktions-E-Mail

-Gültigkeitsüberprüfung des erfassten Textes auf eine gültige E-Mail-Adresse.  
-Mailbenachrichtigung auf diese Funktions-E-Mail erweitern (siehe Fristerinnerungen / Mailbenachrichtigungen)

### Kündigungsfrist

Optional, soll nicht ausgefüllt werden müssen

Besteht aus drei Eigenschaften:

- Kündigungsfrist-Wert (integer)
- Kündigungsfrist-Einheit (Enum): Tage, Wochen, Monat, Jahr
- Kündigungsfrist-Einheits-Ende (Enum): Monatsende, Quartalsende, Jahresende, Vertragsende


### Erklärungsfrist

Optional

Besteht aus drei Eigenschaften:
- Erklärungsfrist-Wert (integer)
- Erklärungsfrist -Einheit (Enum): Tage, Wochen, Monat, Jahr
- Erklärungsfrist -Einheits-Ende (Enum): Monatsende, Quartalsende, Jahresende, Vertragsende

### Vertragsende optional

Das Feld «Ende» (`bisB_ContractEnd`) am Vertrag (Vertragsversion) wird neu optional und kann auch leer gelassen werden. Entsprechend ausgefüllte Vertrage sind unbefristete Dauerschuldverhältnisse (z. B. laufende Wartungsverträge ohne definiertes Enddatum) sowie Verträge über Einzelleistungen, bei denen kein Vertragsende vereinbart ist.

Fehlt das Enddatum, können alle Benachrichtigungstypen, die auf dem Vertragsende basieren (Typ 1 — Vertragsende, Typ 3 — Verlängerungsoption), nicht ausgelöst werden.  Für unbefristete Verträge mit Kündigungsmöglichkeit greift stattdessen ggf. Typ 2 (Benachrichtigung Kündigungsmöglichkeit).

### Beliebige Frist

Gemäss Anforderungen soll es möglich sein beliebige Fristen,  z. B. für Benachrichtigungen über den Ablauf von Gewährleistungs-, Garantie- oder Leistungsfristen oder auch anstehende Prüfungen/Warenabrufe am Vertrag zu führen

### «Ablauf Mangelfrist»

Die bestehende Eigenschaft «Ablauf Mangelfrist» an am Vertrag (Vertragsversion) wird entfernt. Der Begriff «Mangelfrist» ist fachlich nicht korrekt und existiert im Vertragswesen nicht. Gewährleistungs-, Garantie- und Leistungsfristen sowie weitere terminkritische Ereignisse können neu flexibel über Fristerinnerungs-Objekte vom Typ 4 (Beliebige Frist) abgebildet werden; die inhaltliche Beschreibung erfolgt im Feld «Bemerkung» des jeweiligen Objekts.

### «Kosten» (ehem. «Kosten (Jahr)»)

Das Feld «Kosten (Jahr)» wird in «Kosten» umbenannt. Zusätzlich wird die Zeiteinheit als frei wählbares Feld ergänzt (Monat, Quartal, Jahr), sodass die hinterlegten Kosten klar einer Periode zugeordnet werden können. Die Angabe ist rein informativ, es sind keine weiteren Funktionen, Prozesse oder Berechnungen damit geknüpft.

## Validierungen

Gemäss Anforderung Punkt 43 muss jede Eingabe validiert werden.  
Wo nur der Datentyp (Datumswert, Textwert, Ganzzahl, ...) eingehalten werden muss, passiert dies durch das System automatisch.  

Bei spezifischeren Anforderungen wie z.B. bei einem Textwert, der eine E-Mail-Adresse repräsentieren soll, muss dies spezifisch festgelegt werden. 

Aktuell sind folgende spezifische Validierungen auf Grund der Anforderungen vorgesehen:
- Prüfung der Mailadresse bei der Funktions-E-Kail

## Dokumente

Bereits heute erlaubt es das System Dokumente (z.B. Vertragsdokumente) hinzuzufügen, wobei die Dateien an zentraler Stelle abgelegt werden.  
Neu soll es möglich sein auf bestehende Dokumente oder URLs zu verlinken ohne, dass das Dokument selbst Teil des Systems wird.
So können beispielsweise VIS Akte von anderen Systemen direkt verlinkt werden.

# Fristen / Fristerinnerungen

## Idee

Im Vertragsmanagement gibt es verschiedene zeitkritische Ereignisse, über die der Vertragsverantwortliche rechtzeitig informiert werden soll: das Vertragsende, Kündigungsmöglichkeiten, Verlängerungsoptionen sowie beliebige vertragsspezifische Fristen (z.B. Gewährleistungsfristen, Prüftermine). Bisher wurden einzelne dieser Informationen direkt als Felder an der Vertragsversion geführt, ohne dass eine systematische, typisierte Benachrichtigungslogik dahinter stand.

Die neue Lösung basiert auf dem Konzept der **Fristerinnerungs-Unterobjekte**: Statt fixer Felder werden typisierte Objekte angelegt, die einer Vertragsversion untergeordnet sind und die gesamte Benachrichtigungslogik tragen.

### Hierarchie

```
Vertrag (bisB_Contract)
 └── Vertragsversion (bisB_ContractVersion)
      ├── Fristerinnerung Typ 1: Vertragsende
      ├── Fristerinnerung Typ 2: Kündigungsmöglichkeit  (0..n)
      ├── Fristerinnerung Typ 3: Verlängerungsoption    (0..n)
      └── Fristerinnerung Typ 4: Beliebige Frist        (0..n)
```

Die **Vertragsversion** (`bisB_ContractVersion`) ist der fachlich richtige Anknüpfungspunkt, da Laufzeit, Kündigungsregelungen und Verlängerungsbedingungen versionsgebunden sind. Auch das Vertragsende (`bisB_ContractEnd`) ist eine Eigenschaft der Vertragsversion.

Die Fristerinnerungs-Objekte selbst kennen ihren Typ und leiten daraus die Berechnungslogik ab. Auf den übergeordneten Vertragsversion-Eigenschaften (wie Vertragsende, Kündigungsfrist-Felder) berechnen sie ihren Benachrichtigungszeitpunkt und lösen automatisiert E-Mail-Benachrichtigungen aus.

## Fristen

Es werden vier Fristen-Typen unterschieden:

**Typ 1 — Vertragsende:** Erinnert den Vertragsverantwortlichen rechtzeitig vor Ablauf der Vertragslaufzeit. Grundlage ist das Vertragsende (`bisB_ContractEnd`) der übergeordneten Vertragsversion. Pro Vertragsversion kann höchstens ein Objekt dieses Typs angelegt werden.

**Typ 2 — Kündigungsmöglichkeit:** Erinnert an den nächstmöglichen Kündigungszeitpunkt. Der Zeitpunkt wird aus der Kündigungsfrist (Wert, Einheit) sowie dem Einheits-Ende (Monatsende, Quartalsende, Jahresende, Vertragsende) der übergeordneten Vertragsversion berechnet. Sinnvoll vor allem bei Verträgen mit langen Kündigungsfristen oder seltenen Kündigungszeitpunkten.

**Typ 3 — Verlängerungsoption:** Erinnert daran, dass innerhalb einer Erklärungsfrist eine Entscheidung zur Verlängerung oder Nicht-Verlängerung getroffen und kommuniziert werden muss. Der Bezugszeitpunkt (Fristende) ist analog zu Typ 2 über `EinheitsEnde` frei konfigurierbar (Monatsende, Quartalsende, Jahresende oder Vertragsende).

**Typ 4 — Beliebige Frist:** Für beliebige vertragsspezifische Ereignisse, z.B. Ablauf von Gewährleistungs-, Garantie- oder Leistungsfristen, anstehende Prüfungen oder Warenabrufe. Das Fristdatum wird direkt am Objekt gesetzt. Pro Vertragsversion können beliebig viele Typ-4-Objekte angelegt werden.

Die Checkboxen **«ordentliche Kündigung»** und **«Verlängerungsoption»** bleiben als eigenständige Informationsfelder an der Vertragsversion erhalten. Sie dienen der schnellen Übersicht und Filterbarkeit, sind aber unabhängig von der Benachrichtigungslogik: Massgeblich für die Benachrichtigung ist das Vorhandensein der entsprechenden Fristerinnerungs-Unterobjekte (Typ 2 bzw. Typ 3), nicht der Zustand der Checkboxen.

## Modell

Das nachfolgende Modell beschreibt ausschliesslich die **Fristerinnerungs-Unterobjekte**. 

| Eigenschaft                    | Typ     | Pflicht | Beschreibung                                                                   |
| ------------------------------ | ------- | ------- | ------------------------------------------------------------------------------ |
| Typ (`Typ`)                    | Enum    | ja      | Vertragsende \| Kündigungsmöglichkeit \| Verlängerungsoption \| BeliebigeFirst |
| Wert (`Wert`)                  | Integer | nein    | Anzahl Zeiteinheiten der Frist (für Typ 2 und 3)                               |
| Einheit (`Einheit`)            | Enum    | nein    | Tage \| Wochen \| Monat \| Jahr (für Typ 2 und 3)                              |
| Einheits-Ende (`EinheitsEnde`) | Enum    | nein    | Monatsende \| Quartalsende \| Jahresende \| Vertragsende (für Typ 2 und 3)     |
| Datum (`Datum`)                | Date    | nein    | Fristdatum bei direkter Eingabe (für Typ 4)                                    |
| Vorlaufzeit (`Vorlaufzeit`)    | Integer | nein    | Override des systemweiten Defaults in Tagen (alle Typen)                       |
| Bemerkung (`Bemerkung`)        | Text    | nein    | Inhaltliche Beschreibung der Frist (empfohlen für Typ 4)                       |
| Fristbeginn (`FristBeginn`)    | Date    | —       | Berechnet, readonly (siehe Fristberechnung je Typ)                             |
| Fristende (`FristEnde`)        | Date    | —       | Berechnet, readonly (siehe Fristberechnung je Typ)                             |

### Fristberechnung je Typ

**Typ 1 — Vertragsende:**
Leitet sich direkt aus dem Vertragsende (`bisB_ContractEnd`) der übergeordneten Vertragsversion ab. Keine zusätzlichen Eigenschaften am Objekt erforderlich.

- Fristbeginn: `Vertragsende − Vorlaufzeit`
- Fristende: `Vertragsende`

**Typ 2 — Kündigungsmöglichkeit:**
Erfordert `Wert`, `Einheit` und `Einheits-Ende`. Der nächstmögliche Kündigungszeitpunkt bestimmt sich nach `Einheits-Ende`:

- «Monatsende» = letzter Tag des laufenden oder nächsten Monats
- «Quartalsende» = letzter Tag des laufenden oder nächsten Quartals
- «Jahresende» = 31. Dezember des laufenden oder nächsten Jahres
- «Vertragsende» = Vertragsende (`bisB_ContractEnd`) der übergeordneten Vertragsversion

Berechnung:

- Fristbeginn: `nächstmöglicher Kündigungszeitpunkt − Wert (in Einheit)`
- Fristende: `nächstmöglicher Kündigungszeitpunkt`

**Typ 3 — Verlängerungsoption:**
Erfordert `Wert`, `Einheit` und `EinheitsEnde`. Der Bezugszeitpunkt (Fristende) bestimmt sich analog zu Typ 2 nach `EinheitsEnde`:

- «Monatsende» = letzter Tag des laufenden oder nächsten Monats
- «Quartalsende» = letzter Tag des laufenden oder nächsten Quartals
- «Jahresende» = 31. Dezember des laufenden oder nächsten Jahres
- «Vertragsende» = Vertragsende (`bisB_ContractEnd`) der übergeordneten Vertragsversion

Berechnung:

- Fristbeginn: `Bezugszeitpunkt − Wert (in Einheit)`
- Fristende: `Bezugszeitpunkt`

**Typ 4 — Beliebige Frist:**
Erfordert `Datum` (direkte Eingabe). `Vorlaufzeit` und `Bemerkung` sind optional.

- Fristbeginn: `Datum − Vorlaufzeit` (bzw. `Datum − systemweiter Default`, wenn kein Override gesetzt)
- Fristende: `Datum`

### Validierung

- Typ 1 kann pro Vertragsversion nur einmal angelegt werden (da es nur ein Vertragsende (`bisB_ContractEnd`) gibt).
- Typ 2 und 3 können pro Vertragsversion je einmal angelegt werden.
- Typ 4 ist unbeschränkt mehrfach möglich.
- Fehlt das Vertragsende (`bisB_ContractEnd`) an der Vertragsversion bei Typ 1, wird keine Benachrichtigung ausgelöst. Bei Typ 3 gilt dies nur, wenn `EinheitsEnde` auf «Vertragsende» gesetzt ist; bei den übrigen Werten (Monatsende, Quartalsende, Jahresende) ist kein Vertragsende erforderlich.
- Fehlen `Wert`, `Einheit` oder `EinheitsEnde` bei Typ 2/3, wird keine Benachrichtigung ausgelöst.
- Der Benutzer wird in der UI auf fehlende Pflichtfelder hingewiesen.

## Mailbenachrichtigung

Das System sendet automatisierte E-Mail-Benachrichtigungen an den Vertragsverantwortlichen sowie an die hinterlegte Funktions-E-Mail (sofern vorhanden). Es gibt vier Benachrichtigungstypen.

Benachrichtigungen werden als **Sammel-E-Mail pro Empfänger** generiert: Sind an einem Tag mehrere Fristerinnerungen für denselben Empfänger fällig, erhält dieser eine einzige E-Mail mit allen betroffenen Verträgen — nicht eine separate E-Mail pro Vertrag oder Fristerinnerung.

*Beispiel:* Beate Freiberg ist Vertragsverantwortliche für drei Verträge, bei denen heute der Benachrichtigungszeitpunkt erreicht wird. Sie erhält eine einzige E-Mail, die alle drei Fälle zusammenfasst, anstatt drei separate Mails.

### Empfänger

Alle Benachrichtigungen werden an folgende Empfänger versendet:

- Vertragsverantwortlicher (Pflicht)
- Funktions-E-Mail (sofern am Vertrag hinterlegt und gültig; vgl. Abschnitt Funktions-E-Mail)

### Vorlaufzeit

Die Vorlaufzeit bestimmt, wie viele Tage vor dem berechneten Fristzeitpunkt die E-Mail-Benachrichtigung versendet wird (ganzzahliger Wert in Tagen). Sie ist zweistufig konfigurierbar:

- **Systemweiter Default:** Ein einziger Standardwert gilt für alle Benachrichtigungstypen. Dieser wird systemweit hinterlegt und greift, wenn am jeweiligen Fristerinnerungs-Objekt kein individueller Wert gesetzt ist.
- **Override pro Objekt:** Die Vorlaufzeit kann an jedem einzelnen Fristerinnerungs-Objekt individuell überschrieben werden, sofern die standardmässige Vorlaufzeit für den konkreten Fall nicht passt.

### Typ 1: Benachrichtigung Vertragsende

**Priorität:** MUSS

**Voraussetzung:** Fristerinnerung vom Typ 1 ist vorhanden und Vertragsende-Datum (`bisB_ContractEnd`) ist gesetzt.

**Benachrichtigungszeitpunkt:** `Vertragsende − Vorlaufzeit`

**Versand:** Einmalig je Fristerinnerungs-Objekt. Die Kündigungsfrist darf bei dieser Berechnung **nicht** einbezogen werden.

**Hinweis:** Ist das Vertragsende (`bisB_ContractEnd`) an der Vertragsversion nicht gesetzt (unbefristeter Vertrag), erfolgt keine Vertragsende-Benachrichtigung. Stattdessen greift ggf. Typ 2 (Benachrichtigung Kündigungsmöglichkeit).

### Typ 2: Benachrichtigung Kündigungsmöglichkeit

**Priorität:** SOLL

**Voraussetzungen** (alle müssen erfüllt sein):

- Fristerinnerung vom Typ 2 ist vorhanden, UND
- mindestens eine der folgenden Bedingungen ist erfüllt: 
	- Frist > 3 Monate, ODER 
	- Einheits-Ende = «Quartalsende», ODER 
	- Einheits-Ende = «Jahresende»

**Benachrichtigungszeitpunkt:** `nächstmöglicher Kündigungszeitpunkt − Wert (in Einheit) − Vorlaufzeit`

**Hintergrund:** Bei kurzen Kündigungsfristen (z.B. monatlich) ergeben sich häufig neue Kündigungsmöglichkeiten. Die Benachrichtigung soll deshalb nur bei Verträgen mit besonders langen Fristen oder seltenen Kündigungszeitpunkten ausgelöst werden. Fehlen Wert oder Einheit, wird keine Benachrichtigung ausgelöst.

### Typ 3: Benachrichtigung Verlängerungsoption

**Priorität:** MUSS

**Voraussetzungen** (alle müssen erfüllt sein):
- Fristerinnerung vom Typ 3 ist vorhanden, UND
- `Wert`, `Einheit` und `EinheitsEnde` am Objekt sind gesetzt, UND
- Falls `EinheitsEnde` = «Vertragsende»: Vertragsende (`bisB_ContractEnd`) an der übergeordneten Vertragsversion ist gesetzt.

**Benachrichtigungszeitpunkt:** `Bezugszeitpunkt − Wert (in Einheit) − Vorlaufzeit`

**Versand:** Einmalig je Fristerinnerungs-Objekt. Die Benachrichtigung ist zeitkritisch, da innerhalb der Erklärungsfrist eine Entscheidung zur Verlängerung oder Nicht-Verlängerung getroffen und dem Vertragspartner kommuniziert werden muss. Fehlen `Wert`, `Einheit` oder `EinheitsEnde`, wird keine Benachrichtigung ausgelöst.

### Typ 4: Benachrichtigung vertragsspezifische Frist

**Priorität:** MUSS

**Voraussetzung:** Fristerinnerung vom Typ 4 ist vorhanden und `Datum` ist gesetzt.

**Benachrichtigungszeitpunkt:** `Datum − Vorlaufzeit`

**Versand:** Einmalig je Fristerinnerungs-Objekt. Da pro Vertrag mehrere Typ-4-Objekte existieren können, wird für jedes separat eine Benachrichtigung ausgelöst. Die inhaltliche Bedeutung der Frist kann im Feld `Bemerkung` am Objekt hinterlegt werden. Ist `Datum` nicht gesetzt, wird keine Benachrichtigung ausgelöst.

### Zusammenfassung

| Typ                   | Priorität | Bedingung                                                                                                      | Benachrichtigungszeitpunkt                          | Vorlaufzeit                                       |
| --------------------- | --------- | -------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- | ------------------------------------------------- |
| Vertragsende          | MUSS      | Typ-1-Objekt vorhanden + Vertragsende gesetzt                                                                  | `Vertragsende − Vorlaufzeit`                        | Systemweiter Default, override pro Objekt möglich |
| Kündigungsmöglichkeit | SOLL      | Typ-2-Objekt vorhanden + lange Frist/seltener Zeitpunkt                                                        | `nächster Kündigungszeitpunkt − Wert − Vorlaufzeit` | Systemweiter Default, override pro Objekt möglich |
| Verlängerungsoption   | MUSS      | Typ-3-Objekt vorhanden + Wert/Einheit/EinheitsEnde gesetzt (+ Vertragsende, falls EinheitsEnde = Vertragsende) | `Bezugszeitpunkt − Wert − Vorlaufzeit`              | Systemweiter Default, override pro Objekt möglich |
| Beliebige Frist       | MUSS      | Typ-4-Objekt vorhanden + Datum gesetzt                                                                         | `Datum − Vorlaufzeit`                               | Systemweiter Default, override pro Objekt möglich |

# Auftragsformular

## Aktuelles Formular (Ist-Zustand)

```
┌─────────────────────────────────────────────────────────────────────┐
│ Vertrag-Eigenschaften                                               │
├─────────────────────────────────────────────────────────────────────┤
│ Vertrags-Nr.:   [SLT000011      ]   Nummer (alt): [            ]    │
│ Bezeichnung:    [TEST                                           ]   │
│ Bearbeiter:*    [Kostirka, Sven                              ⋯ ]    │
│ Org.-Einheit:*  [■ DER SÄCHSISCHE AUSLÄNDERBEAUFTRAGTE        x ]   │
│ Auftragnehmer:  [■ Byron Informatik AG                     x ]      │
│                 Vertrags-Nr. (AN): [                            ]   │
│ Vertragart:*    [Dienstvertrag                                ▾ ]   │
│ Vertragstyp:    [Dauerschuldverhältnis                        ▾ ]   │
│                 EU-Ausschreibung:* [nein                      ▾ ]   │
├─────────────────────────────────────────────────────────────────────┤
│ Vertragsversionen                                                   │
│ Version:        [■ TEST (SLT000011_2510_0002)             x  ↗ ]    │
│                                                                     │
│  [ Details ] [ Dokumente ] [ zugeo. Verträge ] [ Bearbeiter ]       │
│ ┌────────────────────────────────────────────────────────────────┐  │
│ │  Status:*      [Laufend                                   ▾ ]  │  │
│ │  Abschluss:    [          [D]]                                 │  │
│ │                                                                │  │
│ │   [ Rahmenvertrag ] [ Einzelvertrag ]                          │  │
│ │   Kündigungsfrist:  [3  ] [Monate         ▾]                   │  │
│ │   Verlängerung:     [1  ] [Jahre          ▾]  Automatisch ☑    │  │
│ │                     Begin     Ende   Ablauf Mangelfrist        │  │
│ │   Laufzeit:  [08.04.2025 [D]]  [30.09.2026 [D]]  [  [D]]       │  │
│ │                                                                │  │
│ │   Kosten                                                       │  │
│ │   Kosten (Jahr):    [             ] €  Haushaltstitel: [    ]  │  │
│ │   Zahlungsintervall:[1  ] [Jahre    ▾]                         │  │
│ │                                                                │  │
│ │   Bemerkungen                                                  │  │
│ │   [                                                      ]     │  │
│ └────────────────────────────────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────────────────┤
│ E-Mail-Benachrichtigung                                             │
│ Vorlaufzeit:  [30] Tage im voraus  Vertrag vertraulich: [nein ▾]    │
│ Verantwortlicher: [[P] Freiberg, Beate                      x ]     │
│ Funktions-E-Mail: [                                           ]     │
├─────────────────────────────────────────────────────────────────────┤
│ [ Aktionen ▾ ]                      [ Speichern ] [ Verwerfen ]     │
└─────────────────────────────────────────────────────────────────────┘
```

**Legende:** `[…]` = Eingabefeld · `▾` = Dropdown · `[D]` = Datumsauswahl · `☑` = Checkbox · `x` = Löschen · `⋯` = Bearbeiten · `■` = verknüpftes Objekt · `[P]` = Personenauswahl

### Neues Formular (Soll-Zustand)

Die wichtigsten strukturellen Änderungen gegenüber dem Ist-Zustand:

- Der Block «E-Mail-Benachrichtigung» (Verantwortlicher, Funktions-E-Mail) wird von der Vertragsebene in den **Vertragsversions-Detailblock** verschoben, da diese Angaben versionsgebunden sind.
- Die Tabs «Rahmenvertrag» / «Einzelvertrag» entfallen: Da keine Anforderung diese Unterscheidung verlangt und der Vertragstyp (V2) wegfällt, werden alle Laufzeit-Felder in einem einheitlichen Abschnitt **«Vertragslaufzeit»** zusammengefasst.
- Die bisherigen Einzelfelder für Kündigungsfrist und Verlängerung bleiben als Informationsfelder erhalten, werden aber durch das neue **Fristerinnerungen-Grid** ergänzt.
- Das Fristerinnerungen-Grid erscheint als neuer Abschnitt innerhalb des Vertragsversions-Detailblocks und listet alle angelegten Fristerinnerungs-Objekte auf. Neue Objekte können direkt über ein [+ Hinzufügen]-Element angelegt werden.

```
┌─────────────────────────────────────────────────────────────────────┐
│ Vertrag-Eigenschaften                                               │
├─────────────────────────────────────────────────────────────────────┤
│ Vertrags-Nr.:   [SLT000011      ]   Nummer (alt): [            ]    │
│ Bezeichnung:    [TEST                                           ]   │
│ Bearbeiter:*    [Kostirka, Sven                              ⋯ ]    │
│ Org.-Einheit:*  [■ DER SÄCHSISCHE AUSLÄNDERBEAUFTRAGTE        x ]   │
│ Auftragnehmer:  [■ Byron Informatik AG                     x ]      │
│                 Vertrags-Nr. (AN): [                            ]   │
│ Vertragart:*    [Dienstvertrag                                ▾ ]   │
│                 Ausschreibung:*    [nein                      ▾ ]   │
├─────────────────────────────────────────────────────────────────────┤
│ Vertragsversionen                                                   │
│ Version:        [■ TEST (SLT000011_2510_0002)             x  ↗ ]    │
│                                                                     │
│  [ Details ] [ Dokumente ] [ zugeo. Verträge ] [ Bearbeiter ]       │
│ ┌────────────────────────────────────────────────────────────────┐  │
│ │  Status:*      [Laufend                                   ▾ ]  │  │
│ │  Abschluss:    [          [D]]                                 │  │
│ │                                                                │  │
│ │   Vertragslaufzeit                                             │  │
│ │   Kündigungsfrist:  [3  ] [Monate   ▾]  ord. Kündigung ☑       │  │
│ │   Verlängerung:     [1  ] [Jahre    ▾]  Automatisch ☑          │  │
│ │                     Verlängerungsoption ☑                      │  │
│ │                     Begin          Ende                        │  │
│ │   Laufzeit:  [08.04.2025 [D]]  [30.09.2026 [D]]                │  │
│ │                                                                │  │
│ │   E-Mail-Benachrichtigung                                      │  │
│ │   Verantwortlicher: [[P] Freiberg, Beate                x ]    │  │
│ │   Funktions-E-Mail: [                                   ]      │  │
│ │                                                                │  │
│ │   Kosten                                                       │  │
│ │   Kosten:           [             ] €  [Jahr     ▾]            │  │
│ │   Haushaltstitel:   [    ]                                     │  │
│ │   Zahlungsintervall:[1  ] [Jahre    ▾]                         │  │
│ │                                                                │  │
│ │   Fristerinnerungen                                            │  │
│ │ ┌────────────────────────────────────────────────────────────┐ │  │
│ │ │  Typ                 │ Wert/Datum   │ Einheit │ Bemerk.    │ │  │
│ │ ├────────────────────────────────────────────────────────────┤ │  │
│ │ │  Vertragsende        │ —            │ —       │            │ │  │
│ │ │  Verlängerungsoption │ 3 Mon. vorh. │ Monate  │            │ │  │
│ │ │  Beliebige Frist     │ 15.06.2026   │ —       │ Prüfung    │ │  │
│ │ ├────────────────────────────────────────────────────────────┤ │  │
│ │ │  [+ Fristerinnerung hinzufügen]                            │ │  │
│ │ └────────────────────────────────────────────────────────────┘ │  │
│ │                                                                │  │
│ │   Bemerkungen                                                  │  │
│ │   [                                                      ]     │  │
│ └────────────────────────────────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────────────────┤
│ [ Aktionen ▾ ]                      [ Speichern ] [ Verwerfen ]     │
└─────────────────────────────────────────────────────────────────────┘
```

**Legende:** `[…]` = Eingabefeld · `▾` = Dropdown · `[D]` = Datumsauswahl · `☑` = Checkbox · `x` = Löschen · `⋯` = Bearbeiten · `■` = verknüpftes Objekt · `[P]` = Personenauswahl · `[+ …]` = Neue Zeile hinzufügen

# Ansichten
## Ansicht "Verträge"

Ansichten definieren, welche Eigenschaften in der Tabelle sichtbar sind und wie diese dargestellt werden.  
Im Vertragsmanagement ist nur eine Ansicht namens "Verträge" definiert, welche gemäss nachfolgender Tabelle überarbeitet werden muss.

Wird in den Anforderungen von "Reports" gesprochen wird davon ausgegangen, dass diese Ansicht betroffen ist.

| Eigenschaft IST           | Eigenschaft SOLL                | Autofilter | *Beispiel 1*                          | *Beispiel 2*           |
| ------------------------- | ------------------------------- | ---------- | ----------------------------------- | -------------------- |
| Farbe Auslaufstatus       | Farbe Auslaufstatus             | ja         | *8454016*                             | *16777215*             |
| Standardbezeichnung       | Standardbezeichnung             |            | *SLT000011 TEST*                      | *SLT000009*            |
| Art                       | <fällt weg>                     |            | *Dienstvertrag*                       | *Sonstiges*            |
| Typ                       | <fällt weg>                     |            | *Dauerschuldverhältnis*               |                      |
| —                         | Ausschreibung                   | ja         | *Nein*                                | *Ja*                   |
| verantwortlich            | verantwortlich                  | ja         | *Freiberg, Beate*                     |                      |
| Org-Einheit               | Org-Einheit                     | ja         | *DER SÄCHSISCHE AUSLÄNDERBEAUFTRAGTE* |                      |
| Kunde/Partner             | Kunde/Partner                   | ja         | *Byron Informatik AG*                 | *Byron Informatik AG*  |
| Status                    | Status                          | ja         | *Laufend*                             | *In Vorbereitung*      |
| Haushaltstitel            | Haushaltstitel                  |            |                                     |                      |
| letzte Version            | letzte Version                  |            | *SLT000011_2510_0002*                 | *SLT000009_2503_0001*  |
| Mailwarnung Vertragsende  | <fällt weg>                     |            | *0*                                   | *0*                    |
| Auslaufstatus Vertrag     | Auslaufstatus Vertrag           | ja         | *reguläre Laufzeit*                   | *(ohne Auslaufstatus)* |
| Automatische Verlängerung | Automatische Verlängerung       | ja         | *1*                                   | *1*                    |
| —                         | Beginn                          | ja         | *08.04.2025*                          | *01.01.2020*           |
| —                         | Ende                            | ja         | *30.09.2026*                          | *—*                    |
| —                         | Fristbeginn Vertragsende        | ja         | *01.09.2026*                          | *—*                    |
| —                         | Fristbeginn Kündigung           | ja         | *01.06.2026*                          | *—*                    |
| —                         | Fristbeginn Verlängerungsoption | ja         | *30.06.2026*                          | *—*                    |

Die drei Fristbeginn-Spalten leiten sich aus den Fristerinnerungs-Objekten (Typ 1–3) der letzten Vertragsversion ab. Ist kein entsprechendes Objekt erfasst oder fehlt das Vertragsende, bleibt die Zelle leer.



