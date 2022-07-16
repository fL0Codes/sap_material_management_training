- [Allgemeiner Bestellprozess](#allgemeiner-bestellprozess)
  - [Bestellung anlegen in SAP](#bestellung-anlegen-in-sap)
  - [Wareneingang in SAP](#wareneingang-in-sap)
  - [Bestellhistorie](#bestellhistorie)

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

## Bestellhistorie

Die Historie zu einer Bestellung kann über die Transaktion ME23n unter "Bestellentwicklung" eingesehen werden:

![PO History](/documents/chapter_1/PO_history.png)
