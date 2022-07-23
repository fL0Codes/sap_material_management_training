- [Allgemeiner Bestellprozess](#allgemeiner-bestellprozess)
  - [Bestellung anlegen in SAP](#bestellung-anlegen-in-sap)
  - [Wareneingang in SAP](#wareneingang-in-sap)
  - [Bestellhistorie Materialbeleg](#bestellhistorie-materialbeleg)
  - [Rechnungsprozess](#rechnungsprozess)
  - [Bestellhistorie Rechnungsbeleg](#bestellhistorie-rechnungsbeleg)
  - [Bezahlung der Rechnung](#bezahlung-der-rechnung)
  - [Procure2Pay Zusammenfassung](#procure2pay-zusammenfassung)
- [Unternehmensstruktur und Customizing](#unternehmensstruktur-und-customizing)
  - [Anlage eines neuen Buchungskreises (einer neuen Firmierung)](#anlage-eines-neuen-buchungskreises-einer-neuen-firmierung)
  - [Anlage von weitere Strukturen](#anlage-von-weitere-strukturen)
  - [Testen der neuen Unternehmensstruktur](#testen-der-neuen-unternehmensstruktur)

# Allgemeiner Bestellprozess

Der Bestellprozess (Bestellung = purchase order (PO)) läuft wie folgt ab:

![Purchasing Lifecycle](/documents/chapter_1/purchasing_lifecycle.png)

1. Das Unternehmen gibt eine Bestellung beim Lieferanten auf
2. Der Lieferant liefert die Ware an das Unternehmen (Wareneingang = Goods Receipt (WE/GR))
3. Der Lieferant stellt eine Rechnung (Invoice) an das Unternehmen
4. Das Unternehmen zahlt die Rechnung

Wie anhand des Prozess zu erkennen ist, ist es im B2B-Umfeld so, dass ein Unternehmen meist erst nach Wareneingang und Prüfung der Ware die Ware bezahlt. Im B2C-Umfeld zahlt der Kunde meist erst die Ware und erhält sie erst anschließend.

## Bestellung anlegen in SAP

Über die folgende Transaktion können wir eine PO erstellen:

![PO TCodes](/documents/chapter_1/PO_tcodes.png)

Im nachfolgenden Beispiel nutzen wir die Transaktion ME21N:

![PO Creation](/documents/chapter_1/PO_creation.png)

Bei Bestellungsanlage muss zunächst der Lieferant (Supplier) ausgewählt werden:

![PO Enter Supplier](/documents/chapter_1/PO_enter_supplier.png)

Basierend auf dieser Eingabe werden alle relevanten Informationen zum Lieferanten geladen und es erscheinen weitere, zu befüllende Felder (EK-Org, EK-Gruppe, etc.):

![PO Enter Purchase Departmens](/documents/chapter_1/PO_enter_purchase_departments.png)

Anschließend können die Bestellpositionen mit Materialien befüllt werden, die es zu bestellen gibt:

![PO Enter Line Item](/documents/chapter_1/PO_enter_line_item.png)

**Hinweis:** Basierend auf folgender Kombination kann im IDES eine PO angelegt werden:

![PO Sample IDES7](/documents/chapter_1/PO_sample_ides.png)

Die PO wurde nun erstellt und kann weiter verarbeitet werden, indem sie an den Lieferanten gesendet wird. Hierzu kommt die Nachrichtensteuerung ins Spiel.

In diesem Fall ist die Nachricht der PO so konfiguriert, dass diese ausgedruckt werden soll:

![PO Messages](/documents/chapter_1/PO_messages.png)

Alle mit dieser PO verbundenden Nachrichten werden hier angezeigt:

![PO Message Details](/documents/chapter_1/PO_messages_details.png)

Um die Nachricht auszugeben kann die Transaktion ME9F genutzt werden:

![PO Message Processing](/documents/chapter_1/PO_message_processing.png)

![PO ME9F Entry](/documents/chapter_1/PO_ME9F_entry.png)

![PO ME9F Usage](/documents/chapter_1/PO_ME9F_usage.png)

## Wareneingang in SAP

Nachdem die PO aufgegeben wurde, wird der Lieferant in naher Zukunt die Waren versenden und diese werden angeliefert. Mit der Anlieferung der Ware müssen wir einen Wareneingang erzeugen, um die Waren im SAP zu verbuchen.

Der Wareneingang stellt einen Folgeprozess zur Bestellung dar und kann unter nachfolgendem Ordner im SAP Menü oder über die Transaktion MIGO aufgerufen werden:

![GR Transaction](/documents/chapter_1/GR_transaction.png)

Wareneingänge können aufgrund verschiedener vorheriger Prozesse angelegt werden:

- Anlieferung
- Auftrag
- Auslieferung
- Bestellung
- Materialbeleg
- Reservierung
- Sonstige
- Transport
- Transportidentifikation

Im nachfolgenden Beispiel verbuchen wir den Wareneingang basierend auf der erstellten Bestellung:

![GR Post Goods](/documents/chapter_1/GR_post_ok.png)

Um den WE zu bestätigen, muss für jede Position der Haken bei "Position OK" gesetzt werden.

Nach erfolgreicher Verbuchung erscheint die Meldung, dass der Materialbeleg gebucht wurde.

![GR Post Goods Success](/documents/chapter_1/GR_material_document_success.png)

Der Warenbestand kann anschließend über die Transaktion MMBE geprüft werden:

![GR MMBE Menu](/documents/chapter_1/GR_MMBE_menu.png)

![GR MMBE Entry](/documents/chapter_1/GR_MMBE_entry.png)

![GR MMBE Usage](/documents/chapter_1/GR_MMBE_usage.png)

## Bestellhistorie Materialbeleg

Die Historie zu einer Bestellung kann über die Transaktion ME23n unter "Bestellentwicklung" eingesehen werden:

![PO History](/documents/chapter_1/PO_history.png)

## Rechnungsprozess

Nach der Anlieferung der Waren wird der Lieferant zudem auch eine Rechnung versenden, die beglichen werden muss. Um basierend auf der PO eine Rechnung anzulegen, kann unter den Folgeaktionen zur Bestellung im SAP Menü die Transaktion MIRO ausgewählt werden:

![Invoice Transaction](/documents/chapter_1/IN_MIRO.png)

Innerhalb der Transaktion muss das Rechnungsdatum angegeben werden. Für die Rechnungserstellung kann zudem die PO als Referenz ausgewählt werden. Anschließend werden die PO Positionen inkl. deren Preis in die Rechnung geladen. Nun muss sichergestellt sein, dass stets der gesamte Rechnungsbetrag beglichen wird:

![Invoice Messages](/documents/chapter_1/IN_MIRO_messages.png)

War die Buchung erfolgreich, so wird die nachfolgende Meldung angezeigt zusammen mit der Belegnummer der Rechnung:

![Invoice Document Number](/documents/chapter_1/IN_document_number.png)

## Bestellhistorie Rechnungsbeleg

Auch der Rechnungsbeleg wird in die Bestellhistorie verknüpft:

![PO Invoice History](/documents/chapter_1/PO_history_invoice.png)

## Bezahlung der Rechnung

Das Bezahlen der Rechnung gehört nicht mehr in das SAP MM Modul, sondern ist Teil des SAP FI Moduls. Der Vollständigkeit wegen wird dieser Prozessschritt dennoch aufgeführt.

Der nachfolgende SAP Menüeintrag zeigt, über welche Transaktion eine Bezahlung initiiert werden kann:

![FI SAP Menu](/documents/chapter_1/FI_sap_menu.png)

## Procure2Pay Zusammenfassung

Das nachfolgende Bild zeigt eine Zusammenfassung des Bestellprozess bis hin zur Bezahlung der Rechnung:

![Procure2Pay Cycle](/documents/chapter_1/procure2pay_cycle.png)

# Unternehmensstruktur und Customizing

## Anlage eines neuen Buchungskreises (einer neuen Firmierung)

Innerhalb der SPRO unter dem nachfolgenden Punkt kann zunächst ein neuer Buchungskreis (engl. Company Code) angelegt werden:

![Organizational Structure Company Code](/documents/chapter_2/OrgStruc_company_code.png)

Anschließend erstellen wir einen neuen Buchungskreis indem wir diesen von einem bestehenden kopieren:

![BUKRS Copy](/documents/chapter_2/BUKRS_Copy.png)

Im Hintergrund werden mit den BUKRS verbundenen Daten geladen (das kann einige Zeit in Anspruch nehmen, da viele Daten geladen werden müssen).

Es kann nun ein neuer BUKRS basierend auf einem bestehenden BUKRS angelegt werden.

## Anlage von weitere Strukturen

Anschließend können über weitere Customizing-Leitfäden Geschäftsbereiche, Einkaufsorganisationen, Werke, etc. angelegt werden.
Diese Einträge finden sich ebenfalls im Customizing-Leitfaden für die Unternehmensstruktur. Dort dann unter anderen Subbäumen.

## Testen der neuen Unternehmensstruktur

Um die neue Unternehmensstruktur zu testen kann nun der Bestellprozess für die neu angelegten Struktur-Elemente durchgeführt werden:

Bei der Erstellung einer PO basierend auf den neuen Strukturelementen kommt die nachfolgende Fehlermeldung auf:
![Error PO Creation New EKOrg](/documents/chapter_2/PO_create_new_bukrs_error1.png)

Grundsätzlich ist es im MM-Modul so, dass die Stammdaten stets organisationsbezogen angelegt werden. Demnach muss dem SAP-System klar gemacht werden, dass es einen Lieferanten 1000 auch für die neue EKOrg gibt. Demnach muss der Lieferant zur Lösung der Fehlermeldung neu für die EKOrg angelegt werden. Das geht über Transaktion XK01:

![Vendor Creation - Step 1](/documents/chapter_2/BUKRS_create_new_vendor.png)

Anschließend kann ein neuer Lieferant basierend auf der Vorlage eines bestehenden Lieferanten angelegt werden:

![Vendor Creation - Step 2](/documents/chapter_2/BUKRS_create_new_vendor2.png)

Nun kann die PO für die neue EKOrg und den Lieferanten angelegt werden. Allerdings kommt nun die folgende Fehlermeldung auf:

![Error 2 PO Creation New EKOrg](/documents/chapter_2/PO_create_new_bukrs_error2.png)

Das hängt damit zusammen, dass das Werk nicht der EKOrg zugewiesen ist und diese somit auch keine Bestellungen für dieses Werk aufgeben darf. Eine Zuordnung muss über die SPRO stattfinden, um anschließend das Material in das passende Werk bestellen zu können:

![Assign EKOrg to Plant](/documents/chapter_2/BUKRS_assign_ekorg_to_plant.png)

Außerdem ist kann es sinnvoll sein, die neue EKOrg via Customizing ebenfalls dem entsprechenden BUKRS zuzuweisen:

![Assign EKOrg to BUKRS](/documents/chapter_2/BUKRS_assign_ekorg_to_bukrs.png)

Nachdem dieses Customizing durchgeführt wurde kommt folgede Fehlermeldung, sobald die PO-Position eingetragen wird:

![PO Creation Material Plant Error](/documents/chapter_2/PO_create_new_bukrs_error3.png)

In diesem Fall muss, wie beim Lieferanten, das Material dem neuen Werk zugewiesen werden. Ein neues Material auf werksebene kann über die Transaktion MM01 angelegt werden:

![MM01 New Material Creation - Step 1](/documents/chapter_2/MM01_create_material_for_plant.png)

![MM01 New Material Creation - Step 2](/documents/chapter_2/MM01_create_material_for_plant2.png)

Es muss sichergestellt sein, dass das Material auf allen Ebenen im Materialstamm gepflegt ist. Ansonsten erscheinen Fehlermeldungen wie diese:

![MM01 New Material Creation - Step 3](/documents/chapter_2/MM01_create_material_for_plant3.png)

In diesem Fall muss sichergestellt sein, dass für das Material in der Transaktion MM01 auch Buchhaltungsdaten hinterlegt sind!

Anschließend kann die Bestellung erfolgreich gespeichert werden.

Nun kann via MIGO ein Wareneingang gebucht werden. Dabei kommt es allerdings zu folgendem Fehler:

![Goods Receipt Creation Error](/documents/chapter_2/PO_create_new_bukrs_error4.png)

Um diesen Fehler zu beheben, muss die Transaktion FBN1 aufgerufen werden und dort das Nummernkreisintervall "50" für den entsprechenden Buchungskreis angelegt werden:

![Goods Receipt Creation Error 2](/documents/chapter_2/PO_create_new_bukrs_error5.png)

Bei der anschließenden Erstellung einer Rechnung über die Transaktion "MIRO" erscheint der folgende Fehler:

![Invoice Creation Error](/documents/chapter_2/PO_create_new_bukrs_error6.png)

Hier muss ebenfalls ein neues Nummernkreisintervall in der Transaktion FBN1 angelet werden:

![Invoice Creation Error](/documents/chapter_2/PO_create_new_bukrs_error7.png)
