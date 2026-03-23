# Offene Anforderungen — noch nicht im Pflichtenheft enthalten

Abgleich der Anforderungsdokumente (`CM_Funktionsliste_fuer_neues_Angebot V4.xlsx`, `Vorschlag Vertragslaufzeit.xlsx`, `Berechtigungsmanagement BIS CM V4.xlsx`) mit dem aktuellen Stand von `SLT Vertragsmanagement_Pflichtenheft_V2.md`.

---

## Aus CM_Funktionsliste_fuer_neues_Angebot V4.xlsx

### ✓ Item 28 (MUSS): Feld «Ablauf Mangelfrist» entfernen

Das bestehende Feld «Ablauf Mangelfrist» an der Vertragsversion soll entfernt werden, da es inhaltlich nicht korrekt ist («Mangelfrist» existiert im Vertragswesen nicht) und durch das neue Fristerinnerungs-Objekt Typ 4 (Beliebige Frist) vollständig abgelöst wird.

### ✓ Item 29 (SOLL): Kostenfeld umbenennen und Zeiteinheit frei wählbar machen

Das Feld «Kosten (Jahr)» soll in «Kosten» umbenannt werden. Die Zeiteinheit der Kosten (Monat / Quartal / Jahr) soll frei wählbar sein. Die Angabe ist rein informativ, es sind keine weiteren Funktionen daran geknüpft.

### ✓ Item 26 (MUSS): Vertragsende als optionales Feld

Das Feld «Ende» (Vertragsende) muss explizit leer bleiben können — für unbefristete Dauerschuldverhältnisse sowie Verträge über Einzelleistungen ohne definiertes Enddatum. Fehlt das Enddatum, ist keine Benachrichtigung über das Vertragsende möglich; stattdessen greift ggf. Typ 2 (Kündigungsmöglichkeit). Diese Anforderung ist im Pflichtenheft bisher nur indirekt in den Validierungsregeln erwähnt, nicht als eigenständiger Anpassungsbedarf formuliert.

### ✓ Item 36 (MUSS): Reportfunktion mit Fristen

In der Vertragsübersicht (Listenansicht) müssen die Felder Beginn, Ende und Fristen ergänzt werden. Verträge müssen nach diesen Feldern gefiltert werden können.

### ✓ Item 43 (MUSS): Allgemeine Feldprüfungen bei Eingaben

Eingaben sollen vor Übernahme auf Plausibilität geprüft werden. Beispiel: Ein Datum wie 31.12.9999 darf nicht akzeptiert werden. Im Pflichtenheft ist bisher nur die Gültigkeitsprüfung der Funktions-E-Mail-Adresse erwähnt; eine allgemeine Anforderung an Feldvalidierungen fehlt.

### Item 59 (MUSS): Erklärungsfrist «zum» (Bezugszeitpunkt)

Die Erklärungsfrist (Typ 3 — Verlängerungsoption) soll analog zur Kündigungsfrist einen konfigurierbaren Bezugszeitpunkt erhalten: Monatsende, Quartalsende, Jahresende oder Vertragsende. Das aktuelle Modell für Typ 3 enthält nur `Wert` und `Einheit`, kein `Einheits-Ende`. Zu prüfen, ob dieser Bezugszeitpunkt für die Erklärungsfrist fachlich notwendig ist oder ob «Vertragsende» als fixer Bezugspunkt ausreicht.

---

## Aus Berechtigungsmanagement BIS CM V4.xlsx

Das Berechtigungskonzept ist im Pflichtenheft bisher nicht enthalten. Zu entscheiden: eigenes Dokument oder Integration ins Pflichtenheft.

### Rollenmodell

Vier AD-Sicherheitsgruppen mit folgenden Berechtigungen:

| Rolle | AD-Gruppe | Lesen vertraulich | Schreiben vertraulich | Lesen alle | Schreiben alle | Schreiben zugeordnet | Stornieren alle | Stornieren zugeordnet | Anlegen |
|---|---|---|---|---|---|---|---|---|---|
| Key-User | `CAFM_CM_KeyUsers` | x | x | x | x | — | x | — | x |
| Beschaffungswesen | `CAFM_CM_ProcurmentUsers` | — | — | x | x | — | — | — | x |
| Key-Reader | `CAFM_CM_KeyReader` | x | — | x | — | — | — | — | — |
| Vertragsbearbeiter (VB) | `CAFM_CM_ContractUsers` | — | — | x | — | x | — | x | x |

### Definitionen

**Vertragsverantwortlicher (VV):**
- Person aus AD, Eigenschaft des Vertrags im CM-Tool
- Rein informatives Attribut, kein Einfluss auf Rollen/Rechte
- Empfängt Benachrichtigungen (Fristerinnerungen)
- Org.-Einheit des VV = Org.-Einheit des Vertrags
- Wird durch Key-User, Beschaffungswesen oder VB gesetzt
- VV kann aus einer anderen OE stammen als der VB

**Vertragsbearbeiter (VB):**
- Einer oder mehrere VB je Vertrag
- Ersteller des Vertrags wird automatisch als VB eingetragen
- Jeder VB kann weitere VB aus den schreibberechtigten Rollen hinzufügen
- VB kann aus einer anderen OE stammen als der Vertrag
- Jeder VB kann einen beliebigen VV setzen

### Vertrauliche Verträge

Nur Key-User dürfen vertrauliche Verträge lesen und bearbeiten. Key-Reader dürfen vertrauliche Verträge lesen. Alle anderen Rollen haben keinen Zugriff.

### Löschen / Stornieren

Aus Gründen der Revisionssicherheit sollen Verträge nicht gelöscht, sondern storniert werden.

### Offboarding VB / VV

- Scheidet ein VB oder VV aus dem AD aus, muss er im CM-Tool weiterhin angezeigt werden.
- VB werden durch Key-User manuell geändert (Teil des Offboarding-Prozesses).
- VV werden durch Mitglieder schreibberechtigter Rollen manuell geändert.
- Verträge müssen nach VB und VV gefiltert/gelistet werden können.

### Beendete Verträge

Aus Revisionssicherheit müssen der letzte VB und VV auch bei beendeten Verträgen dauerhaft angezeigt bleiben — auch wenn diese Personen nicht mehr im AD vorhanden sind.
