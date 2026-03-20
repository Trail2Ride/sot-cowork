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

### Vertagsart/-Typ
==Int. Frage: Kann die Differenzierung gestrichen werden?==
> Gemäss Besprechung mit G. Fritsche soll dies mal so probiert werden
#### Vertagsart/-Typ anpassen (V1)

**Vertragstyp** *NEU*

- beinhaltet neu die Enum-Werte der Vertragsart (unbefristet, befristet, ....)
- Soll an die Vertragsversion
- 
**Vertragstyp** *BISHER*
- fällt weg

#### Vertagsart/-Typ fällt weg (V2 bevorzugt)

- Vertragsarten / Typen fallen weg

### Eigenschaft Ausschreibung

- Eigenschaft "EU-Ausschreibung" umbenennen in "Ausschreibung".
- Eigenschaft «Ausschreibung» in Ansicht «Verträge» ergänzen.

### Funktions-E-Mail

-Gültigkeitsüberprüfung des erfassten Textes auf eine gültige E-Mail-Adresse.  
-Mailbenachrichtigung auf diese Funktions-E-Mail erweitern (siehe Fristerinnerungen / Mailbenachrichtigungen)

### ~~Fristerinnerungen~~

~~Kündigungsfrist, Erklärungsfrist und beliebige Fristen werden nicht mehr als direkte Eigenschaften an der Vertragsversion geführt, sondern als **Fristerinnerungs-Unterobjekte** der Vertragsversion (vgl. Kapitel «Umgang mit Fristen / Fristerinnerungen»).~~

~~Die Checkboxen **«ordentliche Kündigung»** und **«Verlängerungsoption»** bleiben als eigenständige Informationsfelder an der Vertragsversion erhalten. Sie dienen der schnellen Übersicht und Filterbarkeit in der Vertragsansicht, unabhängig davon, ob ein entsprechendes Fristerinnerungs-Objekt vorhanden ist. Die Benachrichtigungslogik wertet jedoch nicht die Checkboxen aus, sondern das Vorhandensein der jeweiligen Fristerinnerungs-Unterobjekte (Typ 2 bzw. Typ 3).~~

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

### Beliebige Frist

Gemäss Anforderungen soll es möglich sein beliebige Fristen,  z. B. für Benachrichtigungen über den Ablauf von Gewährleistungs-, Garantie- oder Leistungsfristen oder auch anstehende Prüfungen/Warenabrufe am Vertrag zu führen

# Fristen / Fristerinnerungen

## Fristen

Die Fristberechnung sowie die zugehörigen Benachrichtigungen basieren vollständig auf Fristerinnerungs-Unterobjekten der Vertragsversion. Die Berechnungslogik je Typ ist im Kapitel «Umgang mit Fristen / Fristerinnerungen» beschrieben.

## Umgang mit Fristen / Fristerinnerungen

Fristerinnerungen sind Unterobjekte der **Vertragsversion** (`bisB_ContractVersion`). Da Laufzeit, Kündigungsfrist und Verlängerungsregelungen versionsgebundene Eigenschaften sind, ist es sachlich korrekt, die Fristerinnerungen ebenfalls an der Version zu führen. Pro Vertragsversion können beliebig viele Fristerinnerungen angelegt werden.

### Modell

| Eigenschaft | Typ | Pflicht | Beschreibung |
|---|---|---|---|
| `Typ` | Enum | ja | Vertragsende \| Kündigungsmöglichkeit \| Verlängerungsoption \| BeliebigeFirst |
| `Wert` | Integer | nein | Anzahl Zeiteinheiten der Frist (für Typ 2 und 3) |
| `Einheit` | Enum | nein | Tage \| Wochen \| Monat \| Jahr (für Typ 2 und 3) |
| `Einheits-Ende` | Enum | nein | Monatsende \| Quartalsende \| Jahresende \| Vertragsende (für Typ 2) |
| `Datum` | Date | nein | Fristdatum bei direkter Eingabe (für Typ 4) |
| `Vorlaufzeit` | Integer | nein | Override des systemweiten Defaults in Tagen (nur für Typ 4) |
| `Bemerkung` | Text | nein | Inhaltliche Beschreibung der Frist (empfohlen für Typ 4) |
| `Fristbeginn` | Date | — | Berechnet, readonly (siehe unten) |
| `Fristende` | Date | — | Berechnet, readonly (siehe unten) |

### Fristberechnung je Typ

**Typ 1 — Vertragsende:**
Leitet sich direkt aus dem Vertragsende (`bisB_ContractEnd`) der übergeordneten Vertragsversion ab. Keine zusätzlichen Felder am Objekt erforderlich.

-   Fristbeginn: `Vertragsende − Vorlaufzeit`
-   Fristende: `Vertragsende`

**Typ 2 — Kündigungsmöglichkeit:**
Erfordert `Wert`, `Einheit` und `Einheits-Ende`. Der nächstmögliche Kündigungszeitpunkt bestimmt sich nach `Einheits-Ende`:

-   «Monatsende» = letzter Tag des laufenden oder nächsten Monats
-   «Quartalsende» = letzter Tag des laufenden oder nächsten Quartals
-   «Jahresende» = 31. Dezember des laufenden oder nächsten Jahres
-   «Vertragsende» = Vertragsende (`bisB_ContractEnd`) der übergeordneten Vertragsversion

Berechnung:

-   Fristbeginn: `nächstmöglicher Kündigungszeitpunkt − Wert (in Einheit)`
-   Fristende: `nächstmöglicher Kündigungszeitpunkt`

**Typ 3 — Verlängerungsoption:**
Erfordert `Wert` und `Einheit`. Das Fristende ist implizit das Vertragsende (`bisB_ContractEnd`) der übergeordneten Vertragsversion.

-   Fristbeginn: `Vertragsende − Wert (in Einheit)`
-   Fristende: `Vertragsende`

**Typ 4 — Beliebige Frist:**
Erfordert `Datum` (direkte Eingabe). `Vorlaufzeit` und `Bemerkung` sind optional.

-   Fristbeginn: `Datum − Vorlaufzeit` (bzw. `Datum − systemweiter Default Typ 4`, wenn kein Override gesetzt)
-   Fristende: `Datum`

### Validierung

-   Typ 1 kann pro Vertragsversion nur einmal angelegt werden (da es nur ein Vertragsende (`bisB_ContractEnd`) gibt).
-   Typ 2 und 3 können mehrfach vorkommen, sofern unterschiedliche Fristen vereinbart sind.
-   Typ 4 ist unbeschränkt mehrfach möglich.
-   Fehlt das Vertragsende (`bisB_ContractEnd`) an der Vertragsversion (z.B. unbefristeter Vertrag ohne Enddatum) bei Typ 1 oder 3, wird keine Benachrichtigung ausgelöst. Fehlen `Wert` oder `Einheit` bei Typ 2/3, ebenso. Der Benutzer wird in der UI darauf hingewiesen.

## Mailbenachrichtigungen

Das System sendet automatisierte E-Mail-Benachrichtigungen an den Vertragsverantwortlichen sowie an die hinterlegte Funktions-E-Mail (sofern vorhanden). Benachrichtigungen werden als Sammel-E-Mail pro Empfänger generiert — nicht als Einzel-E-Mail pro Vertrag. Es gibt vier Benachrichtigungstypen.

### Empfänger

Alle Benachrichtigungen werden an folgende Empfänger versendet:

-   Vertragsverantwortlicher (Pflicht)
-   Funktions-E-Mail (sofern am Vertrag hinterlegt und gültig; vgl. Abschnitt Funktions-E-Mail)

### Vorlaufzeit

Die Vorlaufzeit bestimmt, wie viele Tage vor dem berechneten Fristzeitpunkt die E-Mail-Benachrichtigung versendet wird (ganzzahliger Wert in Tagen). Sie ist zweistufig konfigurierbar:

-   **Systemweiter Default pro Typ:** Für jeden der vier Benachrichtigungstypen wird ein eigener Standardwert hinterlegt. Dieser gilt, wenn am jeweiligen Objekt kein individueller Wert gesetzt ist.
-   **Override pro Objekt (Typ 4):** Da beliebige Fristen in Natur und Dringlichkeit stark variieren können, lässt sich die Vorlaufzeit bei Typ-4-Objekten individuell überschreiben.

### Typ 1: Benachrichtigung Vertragsende

**Priorität:** MUSS

**Voraussetzung:** Vertragsende-Datum ist gesetzt.

**Benachrichtigungszeitpunkt:** `Vertragsende − Vorlaufzeit`

**Versand:** Einmalig je Vertrag. Die Kündigungsfrist darf bei dieser Berechnung **nicht** einbezogen werden (bekannter Fehler im bestehenden System: `contract_mail_reminder`).

**Hinweis:** Ist das Vertragsende (`bisB_ContractEnd`) an der Vertragsversion nicht gesetzt (unbefristeter Vertrag), erfolgt keine Vertragsende-Benachrichtigung. Stattdessen greift ggf. Typ 2 (Benachrichtigung Kündigungsmöglichkeit).

### Typ 2: Benachrichtigung Kündigungsmöglichkeit

**Priorität:** SOLL

**Voraussetzungen** (alle müssen erfüllt sein):

-   Fristerinnerung vom Typ 2 ist vorhanden, UND
-   mindestens eine der folgenden Bedingungen ist erfüllt: Frist > 3 Monate, ODER Einheits-Ende = «Quartalsende», ODER Einheits-Ende = «Jahresende»

**Benachrichtigungszeitpunkt:** `nächstmöglicher Kündigungszeitpunkt − Wert (in Einheit) − Vorlaufzeit`

**Hintergrund:** Bei kurzen Kündigungsfristen (z.B. monatlich) ergeben sich häufig neue Kündigungsmöglichkeiten. Die Benachrichtigung soll deshalb nur bei Verträgen mit besonders langen Fristen oder seltenen Kündigungszeitpunkten ausgelöst werden. Fehlen Wert oder Einheit, wird keine Benachrichtigung ausgelöst.

### Typ 3: Benachrichtigung Verlängerungsoption

**Priorität:** MUSS

**Voraussetzungen:** Fristerinnerung vom Typ 3 ist vorhanden, UND Vertragsende (`bisB_ContractEnd`) an der übergeordneten Vertragsversion ist gesetzt, UND `Wert` und `Einheit` am Objekt sind gesetzt.

**Benachrichtigungszeitpunkt:** `Vertragsende − Erklärungsfrist − Vorlaufzeit`

**Versand:** Einmalig je Vertrag. Die Benachrichtigung ist zeitkritisch, da innerhalb der Erklärungsfrist eine Entscheidung zur Verlängerung oder Nicht-Verlängerung getroffen und dem Vertragspartner kommuniziert werden muss. Sind die Erklärungsfrist-Felder leer, wird keine Benachrichtigung ausgelöst.

### Typ 4: Benachrichtigung vertragsspezifische Frist

**Priorität:** MUSS

**Voraussetzung:** Fristerinnerung vom Typ 4 ist vorhanden und `Datum` ist gesetzt.

**Benachrichtigungszeitpunkt:** `Frist (vertragsspezifisch) − Vorlaufzeit`

**Versand:** Einmalig je Fristerinnerungs-Objekt. Da pro Vertrag mehrere Typ-4-Objekte existieren können, wird für jedes separat eine Benachrichtigung ausgelöst. Die inhaltliche Bedeutung der Frist kann im Feld `Bemerkung` am Objekt hinterlegt werden. Ist `Datum` nicht gesetzt, wird keine Benachrichtigung ausgelöst.

### Zusammenfassung

| Typ | Priorität | Bedingung | Benachrichtigungszeitpunkt | Vorlaufzeit |
|---|---|---|---|---|
| Vertragsende | MUSS | Typ-1-Objekt vorhanden + Vertragsende gesetzt | `Vertragsende − Vorlaufzeit` | Default Typ 1 |
| Kündigungsmöglichkeit | SOLL | Typ-2-Objekt vorhanden + lange Frist/seltener Zeitpunkt | `nächster Kündigungszeitpunkt − Wert − Vorlaufzeit` | Default Typ 2 |
| Verlängerungsoption | MUSS | Typ-3-Objekt vorhanden + Vertragsende gesetzt | `Vertragsende − Wert − Vorlaufzeit` | Default Typ 3 |
| Beliebige Frist | MUSS | Typ-4-Objekt vorhanden + Datum gesetzt | `Datum − Vorlaufzeit` | Default Typ 4, override pro Objekt möglich |

## Erfassungsmasken

### Aktuelle Maske (Ist-Zustand)

```
┌─────────────────────────────────────────────────────────────────────┐
│ Vertrag-Eigenschaften                                               │
├─────────────────────────────────────────────────────────────────────┤
│ Vertrags-Nr.:   [SLT000011      ]   Nummer (alt): [              ] │
│ Bezeichnung:    [TEST                                             ] │
│ Bearbeiter:*    [Kostirka, Sven                                ✎ ] │
│ Org.-Einheit:*  [⊞ DER SÄCHSISCHE AUSLÄNDERBEAUFTRAGTE          ✕ ] │
│ Auftragnehmer:  [⊞ Byron Informatik AG                    ✕ ]       │
│                 Vertrags-Nr. (AN): [                              ] │
│ Vertragart:*    [Dienstvertrag                                  ▾ ] │
│ Vertragstyp:    [Dauerschuldverhältnis                          ▾ ] │
│                 EU-Ausschreibung:* [nein                        ▾ ] │
├─────────────────────────────────────────────────────────────────────┤
│ Vertragsversionen                                                   │
│ Version:        [⊞ TEST (SLT000011_2510_0002)               ✕  ⧉ ] │
│                                                                     │
│  [ Details ] [ Vertrags-Dokumente ] [ zugeordnete Verträge ] [ Bearbeiter ] │
│ ┌───────────────────────────────────────────────────────────────┐  │
│ │ Status:*      [Laufend                                     ▾ ] │  │
│ │ Abschluss:    [          📅]                                   │  │
│ │                                                               │  │
│ │  [ Rahmenvertrag ] [ Einzelvertrag ]                          │  │
│ │  Kündigungsfrist:  [3  ] [Monate           ▾]                 │  │
│ │  Verlängerung:     [1  ] [Jahre            ▾]  Automatisch ☑  │  │
│ │                          Begin          Ende    Ablauf Mangelfrist │
│ │  Laufzeit:         [08.04.2025 📅]  [30.09.2026 📅]  [       📅] │
│ │                                                               │  │
│ │  Kosten                                                       │  │
│ │  Kosten (Jahr):    [                 ] €   Haushaltstitel: [  ] │  │
│ │  Zahlungsintervall:[1  ] [Jahre      ▾]                       │  │
│ │                                                               │  │
│ │  Bemerkungen                                                  │  │
│ │  [                                                          ] │  │
│ └───────────────────────────────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────────────────┤
│ E-Mail-Benachrichtigung                                             │
│ Vorlaufzeit:    [30] Tage im voraus   Vertrag ist vertraulich: [nein▾] │
│ Verantwortlicher: [👤 Freiberg, Beate                            ✕ ] │
│ Funktions-E-Mail: [                                               ] │
├─────────────────────────────────────────────────────────────────────┤
│ [ Aktionen ▾ ]                        [ Speichern ] [ Verwerfen ]  │
└─────────────────────────────────────────────────────────────────────┘
```

**Legende:** `[…]` = Eingabefeld · `▾` = Dropdown · `📅` = Datumsauswahl · `☑` = Checkbox · `✕` = Löschen · `✎` = Bearbeiten · `⊞` = verknüpftes Objekt
