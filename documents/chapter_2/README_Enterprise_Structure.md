# Unternehmensstruktur und Customizing

- [Unternehmensstruktur und Customizing](#unternehmensstruktur-und-customizing)
- [Übersetzungen und Abkürzungen](#übersetzungen-und-abkürzungen)
- [Relevante Transaktionen](#relevante-transaktionen)
- [Unternehmensstruktur und Customizing](#unternehmensstruktur-und-customizing-1)
  - [Anlage eines neuen Buchungskreises](#anlage-eines-neuen-buchungskreises)
  - [Anlage von weitere Strukturen](#anlage-von-weitere-strukturen)
  - [Testen der neuen Unternehmensstruktur](#testen-der-neuen-unternehmensstruktur)

# Übersetzungen und Abkürzungen

| Deutsch | Englisch | Abkürzung |
| :------ | :------: | :-------: |

# Relevante Transaktionen

| Transaktion |                       Beschreibung                       |
| :---------- | :------------------------------------------------------: |
| **SPRO**    | Customizing - Projektbearbeitung (Einführungsleitfaden ) |
| **XK01**    |                Anlegen Kreditor (Zentral)                |
| **MM01**    |                    Material & Anlegen                    |
| **FBN1**    |             Nummernkreise Buchhaltungsbeleg              |

# Unternehmensstruktur und Customizing

## Anlage eines neuen Buchungskreises

Über die Transaktion **SPRO** unter dem nachfolgenden Punkt kann ein neuer Buchungskreis angelegt werden:

![Organizational Structure Company Code](/documents/chapter_2/images/OrgStruc_company_code.png)

Anschließend erstellen wir einen neuen Buchungskreis indem wir diesen von einem bestehenden kopieren (am einfachsten für dieses Beispiel):
![BUKRS Copy](/documents/chapter_2/images/BUKRS_Copy.png)

Im Hintergrund werden mit den Buchungskreis verbundenen Daten geladen (das kann einige Zeit in Anspruch nehmen).

Es kann nun ein neuer Buchungskreis basierend auf einem bestehenden Buchungskreis angelegt werden.

Zurück zum [Anfang](#unternehmensstruktur-und-customizing)

## Anlage von weitere Strukturen

Anschließend können über weitere Customizing-Leitfäden Geschäftsbereiche, Einkaufsorganisationen, Werke, etc. angelegt werden.
Diese Einträge finden sich ebenfalls im Customizing-Leitfaden für die Unternehmensstruktur. Dort dann unter anderen Subbäumen.

Zurück zum [Anfang](#unternehmensstruktur-und-customizing)

## Testen der neuen Unternehmensstruktur

Um die neue Unternehmensstruktur zu testen kann nun der Bestellprozess für die neu angelegten Struktur-Elemente durchgeführt werden:

Bei der Erstellung einer Bestellung basierend auf den neuen Strukturelementen kommt die nachfolgende Fehlermeldung auf:
![Error PO Creation New EKOrg](/documents/chapter_2/images/PO_create_new_bukrs_error1.png)

Grundsätzlich ist es im MM-Modul so, dass die Stammdaten stets organisationsbezogen angelegt werden. Demnach muss dem SAP-System so eingestellt werden, dass es einen Lieferanten 1000 auch für die neue Einkaufsorganisation gibt. Daher muss der Lieferant zur Lösung der Fehlermeldung neu für die Einkaufsorganisation angelegt werden. Das geht über Transaktion **XK01**:

![Vendor Creation - Step 1](/documents/chapter_2/images/BUKRS_create_new_vendor.png)

Anschließend kann ein neuer Lieferant basierend auf der Vorlage eines bestehenden Lieferanten angelegt werden:

![Vendor Creation - Step 2](/documents/chapter_2/images/BUKRS_create_new_vendor2.png)

Nun kann die Bestellung für die neue Einkaufsorganisation und den Lieferanten angelegt werden. Anschließend kommt ggf. die nachfolgende Fehlermeldung auf:

![Error 2 PO Creation New EKOrg](/documents/chapter_2/images/PO_create_new_bukrs_error2.png)

Das hängt damit zusammen, dass das Werk nicht der Einkaufsorganisation zugewiesen ist und diese somit auch keine Bestellungen für dieses Werk aufgeben darf. Eine Zuordnung muss über die **SPRO** stattfinden, um anschließend das Material in das passende Werk bestellen zu können:

![Assign EKOrg to Plant](/documents/chapter_2/images/BUKRS_assign_ekorg_to_plant.png)

Außerdem ist kann es sinnvoll sein, die neue Einkaufsorganisation via Customizing ebenfalls dem entsprechenden Buchungskreis zuzuweisen:

![Assign EKOrg to BUKRS](/documents/chapter_2/images/BUKRS_assign_ekorg_to_bukrs.png)

Nachdem dieses Customizing durchgeführt wurde kommt folgede Fehlermeldung, sobald die Bestellposition eingetragen wird:

![PO Creation Material Plant Error](/documents/chapter_2/images/PO_create_new_bukrs_error3.png)

In diesem Fall muss, wie beim Lieferanten, das Material dem neuen Werk zugewiesen werden. Ein neues Material auf werksebene kann über die Transaktion **MM01** angelegt werden:

![MM01 New Material Creation - Step 1](/documents/chapter_2/images/MM01_create_material_for_plant.png)

![MM01 New Material Creation - Step 2](/documents/chapter_2/images/MM01_create_material_for_plant2.png)

Es muss sichergestellt sein, dass das Material auf allen Ebenen im Materialstamm gepflegt ist. Ansonsten erscheinen Fehlermeldungen wie diese:

![MM01 New Material Creation - Step 3](/documents/chapter_2/images/MM01_create_material_for_plant3.png)

In diesem Fall muss sichergestellt sein, dass für das Material in der Transaktion **MM01** auch Buchhaltungsdaten hinterlegt sind!

Anschließend kann die Bestellung erfolgreich gespeichert werden.

Nun kann via **MIGO** ein Wareneingang gebucht werden. Dabei kommt es allerdings zu folgendem Fehler:

![Goods Receipt Creation Error](/documents/chapter_2/images/PO_create_new_bukrs_error4.png)

Um diesen Fehler zu beheben, muss die Transaktion **FBN1** aufgerufen werden und dort das Nummernkreisintervall "50" für den entsprechenden Buchungskreis angelegt werden:

![Goods Receipt Creation Error 2](/documents/chapter_2/images/PO_create_new_bukrs_error5.png)

Bei der anschließenden Erstellung einer Rechnung über die Transaktion **MIRO** erscheint der folgende Fehler:

![Invoice Creation Error](/documents/chapter_2/images/PO_create_new_bukrs_error6.png)

Hier muss ebenfalls ein neues Nummernkreisintervall in der Transaktion **FBN1** angelet werden:

![Invoice Creation Error](/documents/chapter_2/images/PO_create_new_bukrs_error7.png)

Anschließend können wir die Bestellungsnummer im bekannten Feld eingeben, den zu begleichenden Betrag eintragen und die Rechnung speichern.

Abschließend kann über die Transaktion **MMBE** geprüft werden, ob die Materialien auch wirklich eingebucht wurden und nun im Lager vorhanden sind:

![Check Material Booking](/documents/chapter_2/images/BUKRS_mmbe_material.png)

Zurück zum [Anfang](#unternehmensstruktur-und-customizing)
