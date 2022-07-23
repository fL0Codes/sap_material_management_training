#Procure to Pay Process (Bestellprozess)

- [Übersetzungen und Abkürzungen](#übersetzungen-und-abkürzungen)
- [Relevante Transaktionen](#relevante-transaktionen)
- [Allgemeiner Bestellprozess](#allgemeiner-bestellprozess)
  - [Bestellung anlegen in SAP](#bestellung-anlegen-in-sap)
  - [Wareneingang buchen in SAP](#wareneingang-buchen-in-sap)
  - [Prüfung des gebuchten Warenbestands](#prüfung-des-gebuchten-warenbestands)
  - [Bestellhistorie zum Materialbeleg](#bestellhistorie-zum-materialbeleg)
  - [Rechnungsprozess](#rechnungsprozess)
  - [Bestellhistorie Rechnungsbeleg](#bestellhistorie-rechnungsbeleg)
  - [Bezahlung der Rechnung](#bezahlung-der-rechnung)
  - [Procure2Pay Zusammenfassung](#procure2pay-zusammenfassung)

# Übersetzungen und Abkürzungen

| Deutsch      |     Englisch      | Abkürzung |
| :----------- | :---------------: | :-------: |
| Bestellung   |  Purchase Order   |    PO     |
| Wareneingang |   Goods Receipt   |    GR     |
| Rechnung     | Invoice (Receipt) |    IR     |
| Lieferant    |     Supplier      |     -     |

# Relevante Transaktionen

| Transaktion |                       Beschreibung                       |
| :---------- | :------------------------------------------------------: |
| **ME21N**   |       Bestellung anlegen (Lieferant/Werk bekannt)        |
| **ME23N**   |       Bestellung anzeigen (Lieferant/Werk bekannt)       |
| **ME25**    |         Bestellung anlegen (Lieferant unbekannt)         |
| **ME9F**    |           Nachrichten zur Bestellung anzeigen            |
| **MIGO**    |                      Warenbewegung                       |
| **MMBE**    |                    Bestandsübersicht                     |
| **MIRO**    |                Logistik-Rechnungsprüfung                 |
| **F-53**    |                  Zahlungsausgang buchen                  |
| **SPRO**    | Customizing - Projektbearbeitung (Einführungsleitfaden ) |
| **XK01**    |                Anlegen Kreditor (Zentral)                |
| **MM01**    |                    Material & Anlegen                    |
| **FBN1**    |             Nummernkreise Buchhaltungsbeleg              |

# Allgemeiner Bestellprozess

Der Bestellprozess läuft wie folgt ab:

![Purchasing Lifecycle](/documents/chapter_1/images/purchasing_lifecycle.png)

1. Das Unternehmen gibt eine Bestellung beim Lieferanten auf
2. Der Lieferant liefert die Ware an das Unternehmen bucht einen Wareneingang
3. Der Lieferant stellt eine Rechnung (Invoice) an das Unternehmen
4. Das Unternehmen zahlt die Rechnung

Wie anhand des Prozess zu erkennen ist, ist es im B2B-Umfeld so, dass ein Unternehmen meist erst nach Wareneingang und Prüfung der Ware die Ware bezahlt. Im B2C-Umfeld zahlt der Kunde meist erst die Ware und erhält sie anschließend.

Zurück zum [Anfang](#procure-to-pay-process-bestellprozess)

## Bestellung anlegen in SAP

Über die folgende Transaktion können wir eine Bestellung erstellen:
![PO TCodes](/documents/chapter_1/images/PO_tcodes.png)

Im nachfolgenden Beispiel nutzen wir die Transaktion **ME21N**:

![PO Creation](/documents/chapter_1/images/PO_creation.png)

Bei Bestellungsanlage muss zunächst der Lieferant ausgewählt werden:

![PO Enter Supplier](/documents/chapter_1/images/PO_enter_supplier.png)

Basierend auf dieser Eingabe werden alle relevanten Informationen zum Lieferanten geladen und es erscheinen weitere, zu befüllende Felder (EK-Org, EK-Gruppe, etc.):

![PO Enter Purchase Departmens](/documents/chapter_1/images/PO_enter_purchase_departments.png)

Anschließend können die Bestellpositionen mit Materialien befüllt werden, die es zu bestellen gibt:

![PO Enter Line Item](/documents/chapter_1/images/PO_enter_line_item.png)

**Hinweis:** Basierend auf folgender Beispiel-Kombination kann im IDES eine Bestellung angelegt werden:

![PO Sample IDES7](/documents/chapter_1/images/PO_sample_ides.png)

Die Bestellung wurde nun erstellt und kann weiter verarbeitet werden, indem sie an den Lieferanten gesendet wird. Hierzu kommt die Nachrichtensteuerung ins Spiel.

In diesem Fall ist die Nachricht der Bestellung so konfiguriert, dass diese ausgedruckt werden soll:

![PO Messages](/documents/chapter_1/images/PO_messages.png)

Alle mit dieser Bestellung verbundenden Nachrichten werden hier angezeigt:

![PO Message Details](/documents/chapter_1/images/PO_messages_details.png)

Um die Nachricht auszugeben kann die Transaktion **ME9F** genutzt werden:

![PO Message Processing](/documents/chapter_1/images/PO_message_processing.png)

![PO ME9F Entry](/documents/chapter_1/images/PO_ME9F_entry.png)

![PO ME9F Usage](/documents/chapter_1/images/PO_ME9F_usage.png)

Zurück zum [Anfang](#procure-to-pay-process-bestellprozess)

## Wareneingang buchen in SAP

Nachdem die Bestellung aufgegeben wurde, wird der Lieferant in naher Zukunt die bestellte Ware versenden und es findet eine Anlieferung statt. Mit der Anlieferung der Ware wird ein Wareneingang erzeugt, um die Waren im SAP zu verbuchen.

Der Wareneingang stellt einen Folgeprozess zur Bestellung dar und kann unter nachfolgendem Ordner im SAP Menü oder über die Transaktion **MIGO** aufgerufen werden:

![GR Transaction](/documents/chapter_1/images/GR_transaction.png)

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

![GR Post Goods](/documents/chapter_1/images/GR_post_ok.png)

Um den Wareneingang zu bestätigen, muss für jede Position der Haken bei "Position OK" gesetzt werden.

Nach erfolgreicher Verbuchung erscheint die Meldung, dass der Materialbeleg gebucht wurde.

![GR Post Goods Success](/documents/chapter_1/images/GR_material_document_success.png)

Zurück zum [Anfang](#procure-to-pay-process-bestellprozess)

## Prüfung des gebuchten Warenbestands

Der Warenbestand kann anschließend über die Transaktion **MMBE** geprüft werden:

![GR MMBE Menu](/documents/chapter_1/images/GR_MMBE_menu.png)

![GR MMBE Entry](/documents/chapter_1/images/GR_MMBE_entry.png)

![GR MMBE Usage](/documents/chapter_1/images/GR_MMBE_usage.png)

Zurück zum [Anfang](#procure-to-pay-process-bestellprozess)

## Bestellhistorie zum Materialbeleg

Die Historie zu einer Bestellung kann über die Transaktion **ME23N** unter dem Punkt "Bestellentwicklung" eingesehen werden:

![PO History](/documents/chapter_1/images/PO_history.png)

Zurück zum [Anfang](<#procure-to-pay-process-(bestellprozess)>)

## Rechnungsprozess

Nach der Anlieferung der bestellten Ware wird der Lieferant eine Rechnung versenden, die zu begleichen ist. Um basierend auf der Bestellung eine Rechnung anzulegen, kann unter den Folgeaktionen zur Bestellung im SAP Menü die Transaktion **MIRO** ausgewählt werden:

![Invoice Transaction](/documents/chapter_1/images/IN_MIRO.png)

In der Transaktion muss zunächst das Rechnungsdatum angegeben werden. Für die Rechnungserstellung kann zudem die Bestellung als Referenz ausgewählt werden. Anschließend werden die Bestellpositionen und deren Preis in die Rechnung geladen. Nun muss sichergestellt sein, dass der gesamte Rechnungsbetrag beglichen wird:

![Invoice Messages](/documents/chapter_1/images/IN_MIRO_messages.png)

War die Buchung erfolgreich, wird die folgende Meldung angezeigt zusammen mit der Belegnummer der Rechnung:

![Invoice Document Number](/documents/chapter_1/images/IN_document_number.png)

Zurück zum [Anfang](#procure-to-pay-process-bestellprozess)

## Bestellhistorie Rechnungsbeleg

Auch der Rechnungsbeleg wird in die Bestellhistorie verknüpft:

![PO Invoice History](/documents/chapter_1/images/PO_history_invoice.png)

Zurück zum [Anfang](#procure-to-pay-process-bestellprozess)

## Bezahlung der Rechnung

Das Bezahlen der Rechnung gehört nicht mehr in das SAP MM, sondern ist Teil von SAP FI. Der Vollständigkeit wegen wird dieser Prozessschritt dennoch aufgeführt.

Über die Transaktion F-53 kann die Bezahlung gestartet werden kann:

![FI SAP Menu](/documents/chapter_1/images/FI_sap_menu.png)

Damit die Bezahlung erstellt werden kann, muss die Konfiguration des Systems vorgenommen sein, was auf dem genutzten Testsystem nicht der Fall war.

Zurück zum [Anfang](#procure-to-pay-process-bestellprozess)

## Procure2Pay Zusammenfassung

Das nachfolgende Bild zeigt eine Zusammenfassung des Bestellprozesses:

![Procure2Pay Cycle](/documents/chapter_1/images/procure2pay_cycle.png)

Zurück zum [Anfang](#procure-to-pay-process-bestellprozess)
