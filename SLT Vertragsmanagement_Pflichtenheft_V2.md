# Inhaltsverzeichnis

1. [Ausgangslage](#ausgangslage)
2. [Zu klärende Punkte](#zu-klärende-punkte)
3. [Anpassungsbedarf](#anpassungsbedarf)
   - [Firma (Vertragspartner)](#firma-vertragspartner)
   - [Vertragseigenschaften](#vertragseigenschaften)
     - Vertragsart/-Typ
     - Eigenschaft Ausschreibung
     - Funktions-E-Mail
     - Kündigungsfrist
     - Erklärungsfrist
     - Verlängerungszeitraum
     - Checkbox «ordentliche Kündigung»
     - Checkbox «Verlängerungsoption»
     - Vertragsende optional
     - Beliebige Frist
     - «Ablauf Mangelfrist»
     - «Kosten» (ehem. «Kosten (Jahr)»)
   - [Validierungen](#validierungen)
   - [Dokumente](#dokumente)
4. [Fristen / Fristerinnerungen](#fristen--fristerinnerungen)
   - [Idee](#idee) (inkl. Hierarchie)
   - [Fristen](#fristen) (Typ 1–4)
   - [Modell](#modell) (inkl. Fristberechnung je Typ, Validierung)
   - [Mailbenachrichtigung](#mailbenachrichtigung) (Empfänger, Vorlaufzeit, Typ 1–4, Zusammenfassung)
5. [Auftragsformular](#auftragsformular)
   - [Aktuelles Formular (Ist-Zustand)](#aktuelles-formular-ist-zustand)
   - [Neues Formular (Soll-Zustand)](#neues-formular-soll-zustand)
6. [Ansichten](#ansichten)
   - [Ansicht «Verträge»](#ansicht-verträge)
7. [Anforderungszuordnung](#anforderungszuordnung)

---

# Ausgangslage

Das Standardmodul für Byron/BIS wurde beim Landtag Sachsen eingeführt und zum Teil an die Anforderungen des Landtags Sachsen angepasst.

Damit das Modul Vertragsmanagement produktiv eingesetzt werden kann, sind jedoch weitere Anpassungen erforderlich. Dieser Anpassungsbedarf wurde im Rahmen eines Workshops und weiterer interner Abklärungen erhoben.

Dieser Anpassungsbedarf ist in die folgenden Dokumente eingeflossen, welche die Basis dieses Pflichtenhefts bilden:

-   `CM_Funktionsliste_fuer_neues_Angebot V4.xlsx`
-   `Vorschlag Vertragslaufzeit.xlsx`

# Zu klärende Punkte
## Vertragsversion

Frage aus Punkt 45: 
> Gibt es immer eine Vertragsversion? Wie wird die Nummer generiert (Pkt. 45

Es gibt IMMER eine Vertragsversion. Diese wird mit dem Erstellen den Vertrags mit erstellt.  
Folgeversionen müssen durch den Anwender manuell erstellt werden.

Die Nummerierung ist hierbei wie folgt:  
{Vertragsnummer} + "\_" + {Jahreszahl (zweistellig)} + {Monat (zweistellig)} + "\_" + {fortlaufende Nummer (vierstellig)} 

# Anpassungsbedarf
Nachfolgend der Anpassungsbedarf gemäss den in der Ausgangslage geschilderten Dokumenten

## Firma (Vertragspartner)

- Bearbeitbarkeit der Firmen resp. der Assoziation zur Person muss auf Benutzer der Benutzergruppe `CAFM_CM_KeyUsers` beschränkt werden, alle anderen Anwender dürfen die Firma nur Read-Only sehen.
    - selbiges gilt für die neue Eigenschaft "Funktion (externer MA)" an der Person
- Funktion ergänzen, dass von der Firma aus eine neue Person hinzugefügt werden kann.
- Funktion ergänzen, dass von der Firma aus aus einer Lookup-Combobox eine Person mit der Firma verknüpft werden kann.
- Datenmodell erweitern
	-   "Funktion (externer MA)" für die Person

**Info**: Für jede Person kann genau EINE Funktion "Funktion (externer MA)" festgelegt werden. Sollte diese Person mehren Firmen zugeordnet sein, so wäre diese Funktion in allen Firmen dieselbe! 


## Vertragseigenschaften

### Vertragsart/-Typ

#### Vertragsart/-Typ anpassen (Variante 1)

**Vertragstyp**

- Bsp: befristet, unbefristet
- **fällt weg**, ergibt sich aus der Fristenlogik


**Vertragsart**  

- Bsp. Katalog, z.B.  Dienstvertrag, Kaufvertrag, Werkvertrag, 
- **bleibt bestehen**, ist rein informativ

#### Vertragsart/-Typ fällt weg (Variante 2, bevorzugt)

- Vertragsarten / -typen fallen weg

### Eigenschaft Ausschreibung

- Eigenschaft "EU-Ausschreibung" umbenennen in "Ausschreibung".
- Eigenschaft «Ausschreibung» in Ansicht «Verträge» ergänzen.

### Funktions-E-Mail

- Gültigkeitsüberprüfung des erfassten Textes auf eine gültige E-Mail-Adresse aus der SLT-Domain.
- Mailbenachrichtigung auf diese Funktions-E-Mail erweitern (siehe Fristerinnerungen / Mailbenachrichtigungen)

### Kündigungsfrist

Optional, soll nicht ausgefüllt werden müssen

Besteht aus drei Eigenschaften:

- Kündigungsfrist-Wert (integer)
- Kündigungsfrist-Einheit (Enum): Tage, Wochen, Monat, Jahr
- Kündigungsfrist-Einheits-Ende (Enum): Monatsende, Quartalsende, Jahresende, Vertragsende


### Erklärungsfrist

Optional, soll nicht ausgefüllt werden müssen

Besteht aus drei Eigenschaften:

- Erklärungsfrist-Wert (integer)
- Erklärungsfrist-Einheit (Enum): Tage, Wochen, Monat, Jahr
- Erklärungsfrist-Einheits-Ende (Enum): Monatsende, Quartalsende, Jahresende, Vertragsende

### Verlängerungszeitraum

Optional, soll nicht ausgefüllt werden müssen

Besteht aus zwei Eigenschaften:

- Verlängerungszeitraum-Wert (integer)
- Verlängerungszeitraum-Einheit (Enum): Tage, Wochen, Monat, Jahr

### Checkbox «ordentliche Kündigung»

Neue Checkbox an der Vertragsversion, die angibt, ob der Vertrag ordentlich kündbar ist. Das Feld ist rein informativ und dient der schnellen Übersicht. Es hat keinen direkten Einfluss auf die Benachrichtigungslogik — massgeblich für eine Benachrichtigung ist das Vorhandensein eines Fristerinnerungs-Objekts vom Typ 2.

### Checkbox «Verlängerungsoption»

Neue Checkbox an der Vertragsversion, die angibt, ob eine vertragliche Verlängerungsoption besteht. Das Feld ist rein informativ und dient der schnellen Übersicht sowie der Filterbarkeit. Es hat keinen direkten Einfluss auf die Benachrichtigungslogik — massgeblich für eine Benachrichtigung ist das Vorhandensein eines Fristerinnerungs-Objekts vom Typ 3.

### Vertragsende optional

Das Feld «Ende» (`bisB_ContractEnd`) an der Vertragsversion wird neu optional und kann leer gelassen werden. Verträge ohne Enddatum sind typischerweise unbefristete Verträge oder Verträge, bei denen kein festes Vertragsende vereinbart ist.

Fehlt das Enddatum, können alle Benachrichtigungstypen, die auf dem Vertragsende basieren (Typ 1 — Vertragsende; Typ 3 — Verlängerungsoption, sofern `EinheitsEnde` = «Vertragsende»), nicht ausgelöst werden. Typ 3 mit `EinheitsEnde` = «Monatsende», «Quartalsende» oder «Jahresende» ist davon nicht betroffen und kann auch ohne Enddatum ausgelöst werden. Für unbefristete Verträge mit Kündigungsmöglichkeit greift stattdessen ggf. Typ 2 (Benachrichtigung Kündigungsmöglichkeit).

### Beliebige Frist

Gemäss Anforderungen soll es möglich sein, beliebige Fristen z. B. für Benachrichtigungen über den Ablauf von Gewährleistungs-, Garantie- oder Leistungsfristen oder auch anstehende Prüfungen/Warenabrufe am Vertrag zu erfassen.

**Abweichung vom Vorschlag Vertragslaufzeit:** Der Vorschlag sieht ein einzelnes Datumsfeld «Frist (vertragsspezifisch)» im Vertragslaufzeit-Block vor. Dieses wird durch das flexiblere Konzept der Fristerinnerungs-Objekte vom Typ 4 ersetzt, da pro Vertragsversion beliebig viele Fristen mit jeweils eigener Bemerkung erfasst werden können (vgl. Fristerinnerungen-Grid im Soll-Mockup).

### «Ablauf Mangelfrist»

Die bestehende Eigenschaft «Ablauf Mangelfrist» an der Vertragsversion wird entfernt. Der Begriff «Mangelfrist» ist fachlich nicht korrekt und existiert im Vertragswesen nicht. Gewährleistungs-, Garantie- und Leistungsfristen sowie weitere terminkritische Ereignisse können neu flexibel über Fristerinnerungs-Objekte vom Typ 4 (Beliebige Frist) abgebildet werden; die inhaltliche Beschreibung erfolgt im Feld «Bemerkung» des jeweiligen Objekts.

### «Kosten» (ehem. «Kosten (Jahr)»)

Das Feld «Kosten (Jahr)» wird in «Kosten» umbenannt. Zusätzlich wird die Zeiteinheit als frei wählbares Feld ergänzt (Monat, Quartal, Jahr), sodass die hinterlegten Kosten klar einer Periode zugeordnet werden können. Die Angabe ist rein informativ; es sind keine weiteren Funktionen, Prozesse oder Berechnungen daran geknüpft.

## Validierungen

Gemäss Anforderung Punkt 43 muss jede Eingabe validiert werden.  
Wo nur der Datentyp (Datumswert, Textwert, Ganzzahl, ...) eingehalten werden muss, passiert dies durch das System automatisch.  

Bei spezifischeren Anforderungen wie z.B. bei einem Textwert, der eine E-Mail-Adresse repräsentieren soll, muss dies spezifisch festgelegt werden. 

Aktuell sind folgende spezifische Validierungen auf Grund der Anforderungen vorgesehen:
- Prüfung der Mailadresse bei der Funktions-E-Mail

## Dokumente

Bereits heute erlaubt es das System, Dokumente (z.B. Vertragsdokumente) hinzuzufügen, wobei die Dateien an zentraler Stelle abgelegt werden.  
Neu soll es möglich sein, auf bestehende Dokumente oder URLs zu verlinken, ohne dass das Dokument selbst Teil des Systems wird.
So können beispielsweise VIS-Akte von anderen Systemen direkt verlinkt werden.

# Fristen / Fristerinnerungen

## Idee

Im Vertragsmanagement gibt es verschiedene zeitkritische Ereignisse, über die der Vertragsverantwortliche rechtzeitig informiert werden soll: das Vertragsende, Kündigungsmöglichkeiten, Verlängerungsoptionen sowie beliebige vertragsspezifische Fristen (z.B. Gewährleistungsfristen, Prüftermine). Bisher wurden einzelne dieser Informationen direkt als Felder an der Vertragsversion geführt, ohne dass eine systematische, typisierte Benachrichtigungslogik dahinter stand.

Die neue Lösung basiert auf dem Konzept der **Fristerinnerungs-Unterobjekte**: Statt fixer Felder werden typisierte Objekte angelegt, die einer Vertragsversion untergeordnet sind und die gesamte Benachrichtigungslogik tragen.

### Hierarchie

```
Vertrag (bisB_Contract)
 └── Vertragsversion (bisB_ContractVersion)
      ├── Fristerinnerung Typ 1: Vertragsende           (0..1)
      ├── Fristerinnerung Typ 2: Kündigungsmöglichkeit  (0..1)
      ├── Fristerinnerung Typ 3: Verlängerungsoption    (0..1)
      └── Fristerinnerung Typ 4: Beliebige Frist        (0..n)
```

Die **Vertragsversion** (`bisB_ContractVersion`) ist der fachlich richtige Anknüpfungspunkt, da Laufzeit, Kündigungsregelungen und Verlängerungsbedingungen versionsgebunden sind. Auch das Vertragsende (`bisB_ContractEnd`) ist eine Eigenschaft der Vertragsversion.

Die Fristerinnerungs-Objekte selbst kennen ihren Typ und leiten daraus die Berechnungslogik ab. Sie berechnen ihren Benachrichtigungszeitpunkt auf Basis des Vertragsendes der übergeordneten Vertragsversion (Typ 1) bzw. ihrer eigenen konfigurierten Fristparameter (Wert, Einheit, EinheitsEnde für Typ 2/3; Datum für Typ 4) und lösen automatisiert E-Mail-Benachrichtigungen aus.

## Fristen

Es werden vier Fristen-Typen unterschieden:

**Typ 1 — Vertragsende:** Erinnert den Vertragsverantwortlichen rechtzeitig vor Ablauf der Vertragslaufzeit. Grundlage ist das Vertragsende (`bisB_ContractEnd`) der übergeordneten Vertragsversion. Pro Vertragsversion kann höchstens ein Objekt dieses Typs angelegt werden.

**Typ 2 — Kündigungsmöglichkeit:** Erinnert an den nächstmöglichen Kündigungszeitpunkt. Wert, Einheit und Einheits-Ende werden direkt am Fristerinnerungs-Objekt konfiguriert; die Felder «Kündigungsfrist» an der Vertragsversion dienen als informative Referenz für die vertraglichen Bedingungen. Sinnvoll vor allem bei Verträgen mit langen Kündigungsfristen oder seltenen Kündigungszeitpunkten.

**Typ 3 — Verlängerungsoption:** Erinnert daran, dass innerhalb einer Erklärungsfrist eine Entscheidung zur Verlängerung oder Nicht-Verlängerung getroffen und kommuniziert werden muss. Wert, Einheit und Einheits-Ende werden direkt am Fristerinnerungs-Objekt konfiguriert; die Felder «Erklärungsfrist» an der Vertragsversion dienen als informative Referenz für die vertraglichen Bedingungen. Der Bezugszeitpunkt (Fristende) ist über `EinheitsEnde` frei konfigurierbar (Monatsende, Quartalsende, Jahresende oder Vertragsende).

**Typ 4 — Beliebige Frist:** Für beliebige vertragsspezifische Ereignisse, z.B. Ablauf von Gewährleistungs-, Garantie- oder Leistungsfristen, anstehende Prüfungen, Warenabrufe oder Abnahmedaten. Das Fristdatum wird direkt am Objekt gesetzt; die inhaltliche Bedeutung wird im Feld «Bemerkung» beschrieben. Pro Vertragsversion können beliebig viele Typ-4-Objekte angelegt werden.

Die Checkboxen **«ordentliche Kündigung»** und **«Verlängerungsoption»** werden als eigenständige Informationsfelder an der Vertragsversion ergänzt. Sie dienen der schnellen Übersicht und Filterbarkeit, sind aber unabhängig von der Benachrichtigungslogik: Massgeblich für die Benachrichtigung ist das Vorhandensein der entsprechenden Fristerinnerungs-Unterobjekte (Typ 2 bzw. Typ 3), nicht der Zustand der Checkboxen.

## Modell

Das nachfolgende Modell beschreibt ausschliesslich die **Fristerinnerungs-Unterobjekte**. 

| Eigenschaft                    | Typ     | Pflicht | Beschreibung                                                                   |
| ------------------------------ | ------- | ------- | ------------------------------------------------------------------------------ |
| Typ (`Typ`)                    | Enum    | ja      | Vertragsende \| Kündigungsmöglichkeit \| Verlängerungsoption \| BeliebigeFrist |
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
│ Vertrags-Nr.:   [SLT000011      ]   Nummer (alt): [             ]   │
│ Bezeichnung:    [TEST                                           ]   │
│ Bearbeiter:*    [Kostirka, Sven                              ⋯ ]   │
│ Org.-Einheit:*  [■ DER SÄCHSISCHE AUSLÄNDERBEAUFTRAGTE        x ]   │
│ Auftragnehmer:  [■ Byron Informatik AG                        x ]   │
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
│ Verantwortlicher: [[P] Freiberg, Beate                       x ]    │
│ Funktions-E-Mail: [                                            ]    │
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
- Das bisherige Vorlaufzeit-Feld auf Vertragsebene entfällt. Die Vorlaufzeit wird neu systemweit als Default konfiguriert; je Fristerinnerungs-Objekt kann ein individueller Override gesetzt werden (vgl. Abschnitt Vorlaufzeit).

```
┌─────────────────────────────────────────────────────────────────────┐
│ Vertrag-Eigenschaften                                               │
├─────────────────────────────────────────────────────────────────────┤
│ Vertrags-Nr.:   [SLT000011      ]   Nummer (alt): [             ]   │
│ Bezeichnung:    [TEST                                           ]   │
│ Bearbeiter:*    [Kostirka, Sven                              ⋯ ]   │
│ Org.-Einheit:*  [■ DER SÄCHSISCHE AUSLÄNDERBEAUFTRAGTE        x ]   │
│ Auftragnehmer:  [■ Byron Informatik AG                        x ]   │
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
│ │                     zum [Vertragsende ▾]                       │  │
│ │   Erklärungsfrist: [3  ] [Monate   ▾]  zum [Vertragsende ▾]    │  │
│ │   Verlängerung:     [1  ] [Jahre    ▾]  Automatisch ☑          │  │
│ │                     Verlängerungsoption ☑                      │  │
│ │                     Begin          Ende                        │  │
│ │   Laufzeit:  [08.04.2025 [D]]  [30.09.2026 [D]]                │  │
│ │                                                                │  │
│ │   E-Mail-Benachrichtigung                                      │  │
│ │   Verantwortlicher: [[P] Freiberg, Beate                x ]    │  │
│ │   Funktions-E-Mail: [                                     ]    │  │
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

Wird in den Anforderungen von "Reports" gesprochen, wird davon ausgegangen, dass diese Ansicht betroffen ist.

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

# Anforderungszuordnung

Die folgende Tabelle ordnet jede Anforderung aus der `CM_Funktionsliste_fuer_neues_Angebot V4.xlsx` dem jeweiligen Abschnitt dieses Pflichtenhefts zu. Bei Anforderungen mit Status «out of the box» oder «umgesetzt» ist keine Neuentwicklung erforderlich; sie werden der Vollständigkeit halber aufgeführt.

| Nr | Funktion | Prio | Status | Abgedeckt in |
| -- | -------- | ---- | ------ | ------------ |
| 1 | Automatisierte fortlaufende Vertragsnummer | MUSS | umgesetzt | Ist-/Soll-Mockup (Vertrags-Nr.) |
| 2 | Vertragsnummer alt (optionales Feld) | MUSS | umgesetzt | Ist-/Soll-Mockup (Nummer alt) |
| 3 | Bezeichnung des Vertrags | MUSS | out of the box | Ist-/Soll-Mockup (Bezeichnung) |
| 4 | Bearbeiter: automatisiertes Festlegen | MUSS | out of the box | Ist-/Soll-Mockup (Bearbeiter) |
| 5 | Org-Einheit auswählen | MUSS | out of the box | Ist-/Soll-Mockup (Org.-Einheit) |
| 6 | Vertragspartner anlegen | MUSS | out of the box | Anpassungsbedarf → Firma (Vertragspartner) |
| 7 | Personen/Ansprechpartner zu Firma anlegen | MUSS | offen | Anpassungsbedarf → Firma (Vertragspartner) |
| 8 | Auftragnehmer/Vertragspartner auswählen | MUSS | out of the box | Ist-/Soll-Mockup (Auftragnehmer) |
| 9 | Vertragsnummer Auftragnehmer (optionales Feld) | MUSS | umgesetzt | Ist-/Soll-Mockup (Vertrags-Nr. AN) |
| 10 | Vertragsart wählen | MUSS | out of the box | Anpassungsbedarf → Vertragsart/-Typ |
| 11 | Vertragsarten pflegen | MUSS | out of the box | Anpassungsbedarf → Vertragsart/-Typ |
| 12 | «EU-Ausschreibung» umbenennen in «Ausschreibung» | MUSS | offen | Anpassungsbedarf → Eigenschaft Ausschreibung |
| 13 | Vertrag ist vertraulich festlegen | MUSS | out of the box | Ist-Mockup (Vertrag vertraulich) |
| 14 | Vertrauliche Verträge: Berechtigungen | MUSS | out of the box | Berechtigungsmanagement (separates Dokument) |
| 15 | Verantwortlichen für Vertrag festlegen | MUSS | umgesetzt | Soll-Mockup (E-Mail-Benachrichtigung → Verantwortlicher) |
| 16 | Funktions-E-Mail festlegen | MUSS | offen | Anpassungsbedarf → Funktions-E-Mail; Mailbenachrichtigung → Empfänger |
| 17 | Vertragsstatus festlegen | MUSS | out of the box | Ist-/Soll-Mockup (Status) |
| 18 | Abschlussdatum setzen | MUSS | out of the box | Ist-/Soll-Mockup (Abschluss) |
| 19 | Dauerschuldverhältnis/Rahmenvertrag umbenennen | KANN | offen | Anpassungsbedarf → Vertragsart/-Typ; Soll-Zustand (Tabs entfallen) |
| 20 | Kündigungsfrist festlegen (Anzahl und Zeitformat) | MUSS | offen | Anpassungsbedarf → Kündigungsfrist |
| 21 | Kündigungszeitpunkt festlegen (Intervalle) | MUSS | offen | Anpassungsbedarf → Kündigungsfrist (Einheits-Ende) |
| 22 | Benachrichtigung Kündigungsmöglichkeit | SOLL | offen | Mailbenachrichtigung → Typ 2: Benachrichtigung Kündigungsmöglichkeit |
| 23 | Verlängerung auswählen (Anzahl und Zeitformat) | MUSS | out of the box | Anpassungsbedarf → Verlängerungszeitraum; Soll-Mockup |
| 24 | Benachrichtigung Verlängerungsoption | MUSS | offen | Mailbenachrichtigung → Typ 3: Benachrichtigung Verlängerungsoption |
| 25 | Beginn pflegen | MUSS | out of the box | Soll-Mockup (Laufzeit → Begin) |
| 26 | Ende (optionales Feld) | MUSS | offen | Anpassungsbedarf → Vertragsende optional |
| 27 | Abnahmedatum (optionales Feld) | KANN | offen | Anpassungsbedarf → Beliebige Frist; Fristen → Typ 4 |
| 28 | Feld «Ablauf Mangelfrist» entfernen | MUSS | offen | Anpassungsbedarf → «Ablauf Mangelfrist» |
| 29 | «Kosten(Jahr)» umbenennen + Auswahlfeld | SOLL | offen | Anpassungsbedarf → «Kosten» (ehem. «Kosten (Jahr)») |
| 30 | Haushaltstitel | SOLL | out of the box | Soll-Mockup (Haushaltstitel) |
| 31 | Zahlungsintervall (Anzahl und Zeitformat) | MUSS | out of the box | Soll-Mockup (Zahlungsintervall) |
| 32 | Vorlauf für Benachrichtigung festlegen | MUSS | out of the box | Mailbenachrichtigung → Vorlaufzeit |
| 33 | Verlinkung zu VIS-Akte/Vorgang/Dokument | MUSS | out of the box | Anpassungsbedarf → Dokumente |
| 34 | Weitere Bearbeiter hinzufügen | MUSS | umgesetzt | Ist-/Soll-Mockup (Tab «Bearbeiter») |
| 35 | Volltextsuche über alle Felder je Rubrik | MUSS | out of the box | Standardfunktion (keine Anpassung erforderlich) |
| 36 | Reportfunktion generell mit Fristen | MUSS | offen | Ansichten → Ansicht «Verträge» |
| 37 | Feld Vertragstyp entfernen | SOLL | offen | Anpassungsbedarf → Vertragsart/-Typ; Soll-Zustand |
| 38 | Benachrichtigung über Vertragsende | MUSS | offen | Mailbenachrichtigung → Typ 1: Benachrichtigung Vertragsende |
| 39 | Benachrichtigung an Funktions-E-Mail | MUSS | offen | Mailbenachrichtigung → Empfänger |
| 40 | Abfrage ob Vertragsversionen angelegt werden sollen | KANN | out of the box | Standardfunktion (keine Anpassung erforderlich) |
| 41 | Antragsfrist für Verlängerung (Anzahl und Zeitformat) | MUSS | offen | Anpassungsbedarf → Erklärungsfrist; Fristen → Typ 3 |
| 42 | Verträge zuordnen | MUSS | out of the box | Ist-/Soll-Mockup (Tab «zugeo. Verträge») |
| 43 | Feldprüfung vor Eingaben | MUSS | offen | Anpassungsbedarf → Validierungen |
| 44 | Neustrukturierung Anzeige Vertragslaufzeit | MUSS | offen | Auftragsformular → Soll-Zustand; Soll-Mockup |
| 45 | Nummerierung der Vertragsversionen | KANN | ungetestet | Zu klärende Punkte → Vertragsversion |
| 46 | Benachrichtigung vertragsspezifischer Fristablauf | MUSS | offen | Mailbenachrichtigung → Typ 4: Benachrichtigung vertragsspezifische Frist |
| 47 | Neues Feld «Frist (vertragsspezifisch)» | MUSS | offen | Anpassungsbedarf → Beliebige Frist; Fristen → Typ 4; Soll-Mockup (Grid) |
| 48 | Überschrift «Vertragslaufzeit» | MUSS | offen | Soll-Mockup (Vertragslaufzeit) |
| 49 | Dauer (Beginn + Ende) | MUSS | offen | Anpassungsbedarf → Vertragsende optional; Soll-Mockup (Laufzeit) |
| 50 | Checkbox «ordentliche Kündigung» | MUSS | offen | Anpassungsbedarf → Checkbox «ordentliche Kündigung»; Soll-Mockup |
| 51 | Feld Kündigungsfrist (Zahl) | MUSS | offen | Anpassungsbedarf → Kündigungsfrist (Wert) |
| 52 | Feld Kündigungsfrist (Zeitformat) | MUSS | offen | Anpassungsbedarf → Kündigungsfrist (Einheit) |
| 53 | Feld Kündigungsfrist «zum» (Kündigungszeitpunkt) | MUSS | offen | Anpassungsbedarf → Kündigungsfrist (Einheits-Ende); Soll-Mockup |
| 54 | Checkbox «Verlängerungsoption» | MUSS | offen | Anpassungsbedarf → Checkbox «Verlängerungsoption»; Soll-Mockup |
| 55 | Verlängerungszeitraum (Zahl) | MUSS | offen | Anpassungsbedarf → Verlängerungszeitraum (Wert) |
| 56 | Verlängerungszeitraum (Zeitformat) | MUSS | offen | Anpassungsbedarf → Verlängerungszeitraum (Einheit) |
| 57 | Erklärungsfrist (Zahl) | MUSS | offen | Anpassungsbedarf → Erklärungsfrist (Wert) |
| 58 | Erklärungsfrist (Zeitformat) | MUSS | offen | Anpassungsbedarf → Erklärungsfrist (Einheit) |
| 59 | Erklärungsfrist «zum» (Zeitpunkt) | MUSS | offen | Anpassungsbedarf → Erklärungsfrist (Einheits-Ende); Soll-Mockup |
