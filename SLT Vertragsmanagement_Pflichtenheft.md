# Ausgangslage

Das Standardmodul für Byron/BIS wurde beim Landtag Sachen eingeführt und zum Teil an die Anforderungen des Landtag Sachsen angepasst.

Damit das Modul Vertragsmanagement produktiv eingesetzt werden kann, sind jedoch weitere Anpassungen erforderlich. Dieser Anpassungsbedarf wurde im Rahmen eines Workshops und weiteren internen Abklärungen erhoben.

Dieser Anpassungsbedarf ist in die folgenden Dokumente eingeflossen, welche die Basis dieses Pflichtenhefts bilden:

-   CM_Funktionsliste_fuer_neues_Angebot V4.xlsx
-   Vorschlag Vertragslaufzeit.xlsx

# Anpassungsbedarf

## Firma (Vertragspartner)

Bearbeitbarkeit der Firmen resp. der Assoziation zur Person müssen auf Benutzer der Benutzergruppe `CAFM_CM_KeyUsers` beschränkt werden, alle anderen Anwender dürfen die Firma nur Read-Only sehen.

Funktion ergänzen, dass von der Firma aus eine neue Person hinzugefügt werden kann.

Funktion ergänzen, dass von der Firma aus aus einer Lookup-Combobox eine Person mit der Firma verknüpft werden kann.

Datenmodell erweitern

-   V1: Funktion für die Person
-   V2: Funktion für Zwischenobjekt `PersonToFirma`

==Frage: Muss die Funktion der Person je Firma individuell festgelegt werden können, oder ist die Funktion der Person generell. Ersteres wäre deutlich aufwändiger in der Umsetzung.==

## Vertragseigenschaften

### Eigenschaft Ausschreibung

Eigenschaft "EU-Ausschreibung" umbenennen in "Ausschreibung".
Eigenschaft «Ausschreibung» in Ansicht «Verträge» ergänzen.

### Funktions-E-Mail

Gültigkeitsüberprüfung des erfassten Textes auf eine gültige E-Mail-Adresse.  
Mailbenachrichtigung auf diese Funktions-E-Mail erweitern

### Vertragstyp

==Int. Frage: Kann die Differenzierung gestrichen werden?==

### Kündigungsfrist

Optional, soll nicht ausgefüllt werden müssen

Besteht aus drei Eigenschaften:

-   Kündigungsfrist-Wert (integer)
-   Kündigungsfrist-Einheit (Enum): Tage, Wochen, Monat, Jahr
-   Kündigungsfrist-Einheits-Ende (Enum): Monatsende, Quartalsende, Jahresende, Vertragsende

### Erklärungsfrist

Optional

Besteht aus drei Eigenschaften:

-   Erklärungsfrist-Wert (integer)
-   Erklärungsfrist -Einheit (Enum): Tage, Wochen, Monat, Jahr
-   Erklärungsfrist -Einheits-Ende (Enum): Monatsende, Quartalsende, Jahresende, Vertragsende

## Vertragsversion

==Int. Frage: Gibt es immer eine Vertragsversion? Wie wird die Nummer generiert (Pkt. 45)==

## Fristen

### Kündigungsfrist

Fristbeginn: Ist die Kündigungsfrist gesetzt (mit Wert, Einheit und Einheits-Ende), so ergibt sich der Fristbeginn aus dem nächstmöglichen Kündigungszeitpunkt abzüglich der Kündigungsfrist. Der nächstmögliche Kündigungszeitpunkt bestimmt sich nach dem konfigurierten Einheits-Ende:

-   «Monatsende» = letzter Tag des laufenden oder nächsten Monats
-   «Quartalsende» = letzter Tag des laufenden oder nächsten Quartals
-   «Jahresende» = 31. Dezember des laufenden oder nächsten Jahres
-   «Vertragsende» = Datum des Vertragsendes

Sind die Felder der Kündigungsfrist leer (nicht gesetzt), wird keine Fristberechnung und keine Benachrichtigung ausgelöst.

### Erklärungsfrist

Erklärungsfrist: falls es eine Verlängerungsoption gibt, muss innerhalb dieser Frist die Option wahrgenommen oder abgelehnt werden.

Fristbeginn: `Vertragsende − Erklärungsfrist`

Sind die Felder der Erklärungsfrist leer (nicht gesetzt), wird keine Fristberechnung und keine Benachrichtigung ausgelöst.

### Beliebige Frist

Beliebig festlegbare Frist (z.B. für Gewährleistungs-, Garantie- oder Leistungsfristen oder auch anstehende Prüfungen/Warenabrufe).

Fristbeginn: frei konfigurierbares Datum (direkte Eingabe).

Die inhaltliche Bedeutung der Frist kann im Feld «Bemerkungen» hinterlegt werden.

## Umgang mit Fristen / Fristerinnerungen

Fristerinnerungen sind Unterobjekte eines Vertrags, unabhängig von der Vertragsversion

### Modell

-   Art (Enum): Kündigungsfrist, Erklärungsfrist, beliebige Frist
-   Zeitraum
    -   Anzahl (Integer)
    -   Einheit (Enum): Tage, Wochen, Monat, Jahr
    -   Einheits-Ende (Enum): Monatsende, Quartalsende, Jahresende, Vertragsende
-   Fristbeginn
    -   Kündigungsfrist: `nächstmöglicher Kündigungszeitpunkt − Kündigungsfrist`
    -   Erklärungsfrist: `Vertragsende − Erklärungsfrist`
    -   Beliebige Frist: Freie Eingabe (direkt als Datum)
-   Fristende
    -   Kündigungsfrist: nächstmöglicher Kündigungszeitpunkt (gemäss Einheits-Ende)
    -   Erklärungsfrist: Vertragsende
    -   Beliebige Frist: konfiguriertes Datum (freie Eingabe)

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

**Hinweis:** Ist kein Vertragsende gesetzt (unbefristeter Vertrag), erfolgt keine Vertragsende-Benachrichtigung. Stattdessen greift ggf. Typ 2 (Benachrichtigung Kündigungsmöglichkeit).

### Typ 2: Benachrichtigung Kündigungsmöglichkeit

**Priorität:** SOLL

**Voraussetzungen** (alle müssen erfüllt sein):

-   Checkbox «ordentliche Kündigung» ist aktiviert, UND
-   mindestens eine der folgenden Bedingungen ist erfüllt: Kündigungsfrist > 3 Monate, ODER Einheits-Ende = «Quartalsende», ODER Einheits-Ende = «Jahresende»

**Benachrichtigungszeitpunkt:** `nächstmöglicher Kündigungszeitpunkt − Kündigungsfrist − Vorlaufzeit`

**Hintergrund:** Bei kurzen Kündigungsfristen (z.B. monatlich) ergeben sich häufig neue Kündigungsmöglichkeiten. Die Benachrichtigung soll deshalb nur bei Verträgen mit besonders langen Fristen oder seltenen Kündigungszeitpunkten ausgelöst werden. Sind die Kündigungsfrist-Felder leer, wird keine Benachrichtigung ausgelöst.

### Typ 3: Benachrichtigung Verlängerungsoption

**Priorität:** MUSS

**Voraussetzungen:** Checkbox «Verlängerungsoption» ist aktiviert, UND Vertragsende-Datum ist gesetzt, UND Erklärungsfrist-Felder sind gesetzt.

**Benachrichtigungszeitpunkt:** `Vertragsende − Erklärungsfrist − Vorlaufzeit`

**Versand:** Einmalig je Vertrag. Die Benachrichtigung ist zeitkritisch, da innerhalb der Erklärungsfrist eine Entscheidung zur Verlängerung oder Nicht-Verlängerung getroffen und dem Vertragspartner kommuniziert werden muss. Sind die Erklärungsfrist-Felder leer, wird keine Benachrichtigung ausgelöst.

### Typ 4: Benachrichtigung vertragsspezifische Frist

**Priorität:** MUSS

**Voraussetzung:** Datumsfeld «Frist (vertragsspezifisch)» ist gesetzt.

**Benachrichtigungszeitpunkt:** `Frist (vertragsspezifisch) − Vorlaufzeit`

**Versand:** Einmalig je Vertrag. Dieses Feld dient als flexibler Auslöser für beliebig konfigurierbare Fristen (z.B. Gewährleistungs-, Garantie-, Leistungsfristen, anstehende Prüfungen oder Warenabrufe). Die inhaltliche Bedeutung der Frist kann im Feld «Bemerkungen» hinterlegt werden. Ist das Feld leer, wird keine Benachrichtigung ausgelöst.

### Zusammenfassung

| Typ | Priorität | Bedingung | Benachrichtigungszeitpunkt | Vorlaufzeit |
|---|---|---|---|---|
| Vertragsende | MUSS | Vertragsende gesetzt | `Vertragsende − Vorlaufzeit` | Default Typ 1 |
| Kündigungsmöglichkeit | SOLL | Checkbox «ord. Kündigung» + lange Frist/seltener Zeitpunkt | `nächster Kündigungszeitpunkt − Kündigungsfrist − Vorlaufzeit` | Default Typ 2 |
| Verlängerungsoption | MUSS | Checkbox «Verlängerungsoption» + Vertragsende + Erklärungsfrist | `Vertragsende − Erklärungsfrist − Vorlaufzeit` | Default Typ 3 |
| Vertragsspez. Frist | MUSS | Frist (vertragsspezifisch) gesetzt | `Frist (vertragsspezifisch) − Vorlaufzeit` | Default Typ 4, override pro Objekt möglich |
