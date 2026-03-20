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

## Idee
==> Idee mit den Fristen / Fristenerinnerungen beschreiben
==> Idee mit der Vertrag, Vertragsversion und Fristerinnerungen (hierarchie) beschreiben

## Fristen
==> Beschreibung der Fristen

## Modell
==> Datenmodell der Firsterinnerungen (NICHT von der vom Vertrag oder der Vertragsversion, da dies schon umgesetzt ist )

## Mailbenachrichtigung
=> Ausführen wann und an wen eine Mailbenachrichtigung erfolgt

## Mockup des Auftragsformulars
==> Darstellung des neuen Auftragsformulars
===> Hierbei sollen die Erinnerungsobjekte als Datensätze in einem Grid irgendwo in der Vertragsversion aufgenommen werden
===> beachten, dass gewisse Informationen neu nicht mehr zu Vertrag sondern zur Vertragsversion gehören