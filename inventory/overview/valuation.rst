:code-column:

===================
Inventory Valuation
===================

Every year your inventory valuation has to be recorded in your balance sheet. This implies two main choices:
  * the way you compute the cost of your stored items (standard vs average vs real price);
  * the way you record the inventory value into your books (periodic vs perpetual).

Costing Method
==============

International accounting standards define several ways to compute product costs:

.. rst-class:: alternatives doc-aside

Standard Price
  .. rst-class:: values-table

  .. list-table::
     :widths: 28 18 18 18 18
     :header-rows: 1
     :stub-columns: 1

     * - Operation
       - Unit Cost
       - Qty On Hand
       - Delta Value
       - Inventory Value
     * -
       - $10
       - 0
       -
       - $0
     * - Receive 8 Products at $10
       - $10
       - 8
       - +8*$10
       - $80
     * - Receive 4 Products at $16
       - $10
       - 12
       - +4*$10
       - $120
     * - Deliver 10 Products
       - $10
       - 2
       - | -10*$10
         |
       - $20
     * - Receive 2 Products at $9
       - $10
       - 4
       - +2*$10
       - $40
"Standard Price" means you estimate the cost price based on direct materials, direct labor and manufacturing overhead at the end of a specific period (usually once a year). You enter this cost price in the product form.

Tip: In case you use Odoo Manufacturing, Odoo embeds a tool to compute this cost automatically from bills of materials and tied work centers (see: https://www.odoo.com/apps/modules/online/product_extended/).
  
Average Price
  .. rst-class:: values-table

  .. list-table::
     :widths: 28 18 18 18 18
     :header-rows: 1
     :stub-columns: 1

     * - Operation
       - Unit Cost
       - Qty On Hand
       - Delta Value
       - Inventory Value
     * -
       - $0
       - 0
       -
       - $0
     * - Receive 8 Products at $10
       - $10
       - 8
       - +8*$10
       - $80
     * - Receive 4 Products at $16
       - $12
       - 12
       - +4*$16
       - $144
     * - Deliver 10 Products [#average-removal]_
       - $12
       - 2
       - | -10*$12
         |
       - $24
     * - Receive 2 Products at $6
       - $9
       - 4
       - +2*$6
       - $36
The "Average Price" method recomputes the cost price as you as a receipt (incoming shipment) has been processed, based on prices defined in tied purchase orders. This method is mainly justified in case of huge purchase price variations. It is quite unusual due to its operational complexity. Your acutally need a software like Odoo to keep this cost up-to-date.

We advise this method for advanced users only. It requires well established business processes. In Odoo, the order in which you process incoming shipments matter. Moreover if you mistakenly process an incoming shipment that you needed to cancel, you have to recover manually by resetting the cost price as it was before.

   .. placeholder

.. [#average-removal] products leaving the stock have no impact on the average price.
       
FIFO
  .. rst-class:: values-table

  .. list-table::
     :widths: 28 18 18 18 18
     :header-rows: 1
     :stub-columns: 1

     * - Operation
       - Unit Cost
       - Qty On Hand
       - Delta Value
       - Inventory Value
     * -
       - $0
       - 0
       -
       - $0
     * - Receive 8 Products at $10
       - $10
       - 8
       - +8*$10
       - $80
     * - Receive 4 Products at $16
       - $12
       - 12
       - +4*$16
       - $144
     * - Deliver 10 Products
       - $16
       - 2
       - | -8*$10
         | -2*$16
       - $32
     * - Receive 2 Products at $6
       - $11
       - 4
       - +2*$6
       - $44
For “Real Price” (FIFO, LIFO, etc), the costing is further refined by the removal strategy set on the warehouse location or product's internal category. The default strategy is FIFO. With such method, your inventory value is based on the value of your quants (see related documentation) and not on the cost price shown in the product form. Whenever you ship items, the cost price is reset to the cost of the last quant shipped. This cost price is used to value any quant generated from stock moves not tied to purchase orders (e.g. inventory adjustments).

Such a method is advised if you manage all your workflow into Odoo (Sales, Purchases, Inventory). It suits any kind of users.

Tip: Odoo allows to split landed costs (transportation, etc.) into the cost of those quants (see: https://www.odoo.com/apps/modules/online/stock_landed_costs/).

LIFO (not accepted in IFRS)
  .. rst-class:: values-table

  .. list-table::
     :widths: 28 18 18 18 18
     :header-rows: 1
     :stub-columns: 1

     * - Operation
       - Unit Cost
       - Qty On Hand
       - Delta Value
       - Inventory Value
     * -
       - $0
       - 0
       -
       - $0
     * - Receive 8 Products at $10
       - $10
       - 8
       - +8*$10
       - $80
     * - Receive 4 Products at $16
       - $12
       - 12
       - +4*$16
       - $144
     * - Deliver 10 Products
       - $10
       - 2
       - | -4*$16
         | -6*$10
       - $20
     * - Receive 2 Products at $6
       - $8
       - 4
       - +2*$6
       - $32

[To add in the left-side column] Odoo allows any method. The default one is "Standard Price". To change it, check *Use a 'Fixed', 'Real' or 'Average' price costing method* in Purchase settings. Then set the costing method from products' internal categories. Categories shows up in the Inventory tab of the product form.

Whatever the method is, Odoo provides a full inventory valuation in *Inventory > Reports > Inventory Valuation* (i.e. current qty in stock * cost price).

For more comparison of those methods: http://www.principlesofaccounting.com/chapter8/chapter8.html#Inventory

Periodic Inventory Valuation
============================

In a periodic inventory valuation, goods reception and outgoing shipments have no direct impact in the accounting. At the end of the month or year, the accountant
posts one journal entry representing the value of the physical inventory. This is the default configuration in Odoo and it works out-of-the-box. Check following operations and find out how Odoo is managing the accounting postings.

.. rst-class:: alternatives doc-aside

Supplier Invoice
  .. rst-class:: values-table

  ============================= ===== ======
  \                             Debit Credit
  ============================= ===== ======
  Expenses: Purchased Goods        50
  Assets: Deferred Tax Assets    4.68
  Liabilities: Accounts Payable	       54.68
  ============================= ===== ======

  Configuration:
    * Purchased Goods: defined on the product or on the internal category of related product (Expense Account field)
    * Deferred Tax Assets: defined on the tax used on the purchase order line
    * Accounts Payable: defined on the vendor related to the bill
Goods Receptions
  No Journal Entry
Customer Invoice
  .. rst-class:: values-table

  ===================================== ===== ======
  \                                     Debit Credit
  ===================================== ===== ======
  Revenues: Sold Goods                           100
  Liabilities: Deferred Tax Liabilities            9
  Assets: Accounts Receivable             109
  ===================================== ===== ======

  Configuration:
    * Revenues: defined on the product or on the internal category of related product (Income Account field)
    * Deferred Tax Liabilities: defined on the tax used on the invoice line
    * Accounts Receivable: defined on the customer (Receivable Account)

  The fiscal position used on the invoice may have a rule that replaces the
  Income Account or the tax defined on the product by another one.
Customer Shipping
  No Journal Entry
Manufacturing Orders
  No Journal Entry
  
.. raw:: html

   <hr style="float: none; visibility: hidden; margin: 0;">

At the end of the month/year, your company does a physical inventory or just relies on the inventory in Odoo to value the stock into your books.

Continental Accounting
----------------------
As a company following the Continental accounting principles (purchase expenses recorded to books when getting products into stock), create a journal entry to move the stock variation value from your P&L to your assets. 

.. h:div:: doc-aside

   If the stock value increased since the last report, the accountant records the following entry:

   .. rst-class:: values-table

  ===================================== ===== ======
  \                                     Debit Credit
  ===================================== ===== ======
  Assets: Inventory                         X     
  Expenses: Inventory Variations                   X            
  ===================================== ===== ======

   If the stock value decreased, he makes it upside down.
   
.. raw:: html

   <hr style="float: none; visibility: hidden; margin: 0;">

Anglo-Saxon Accounting
----------------------
If your company relies on Anglo-Saxons accounting principles (purchase expenses recorded to books when consuming products or sending to customers), your accountant has to break down the purchase balance into both the inventory and the cost of goods sold.

Cost of goods sold (COGS) = Starting inventory value + Purchases – Closing inventory value

To update the stock valuation in your books, record such an entry:

.. h:div:: doc-aside

   .. rst-class:: values-table

  ===================================== ===== ======
  \                                     Debit Credit
  ===================================== ===== ======
  Assets: Inventory (closing value)         X     
  Expenses: Cost of Good Sold               X
  Expenses: Purchased Goods                        X
  Assets: Inventory (starting value)               X            
  ===================================== ===== ======

Perpetual Inventory Valuation
=============================

In a perpetual inventory valuation, goods reception and outgoing shipments are directly posted in your books. The inventory valuation and the cost of good sold (if Anglo-Saxon accounting) is always up-to-date. This mode is advised for expert accountants and advanced users only. As opposed to periodic valuation, it requires some extra configuration. Moreover, the system populates your books with a potential high number of journal entries. A review process is needed before going live to make sure that those automated entries fit your expetations for every single step of your business workflow.

Let's consider the case of retail products.

Continental Accounting
----------------------
new tab with chart of account slightly adapted:
- replace $ by € in the left-side column
- Under Expenses CoA section: replace the accounts by: 51000 Purchased Goods / 52000 Purchased Services / 58000 Inventory Variations / 59000 Other Operating Expenses
- remove 23000 Goods Received Not Purchased &
- error to fix: Supplier Invoice (PO $50, Invoice $40)    -> Supplier Invoice (PO $50, Invoice $50)
- remove 23000 Goods Received Not Purchased & 14600 Goods Issued Not Invoiced (only for Anglo-Saxon)

Changes in transactions: 
 Supplier Invoice (PO €50, Invoice €50): replace 23000 Goods Received Not Purchased 50    by    51000 Purchased Goods  50
 Supplier Goods Reception (PO €50, Invoice €50): 14000 Inventory 50  to   58000 Inventory Variations 50
 Supplier Invoice (PO €48, Invoice €50): 51000 Purchased Goods 48 & 19000 Deferred Tax Assets 4.50    to     21000 Accounts Payable 54.50
 Supplier Goods Reception (PO €48, Invoice €50): 14000 Inventory 48  to   58000 Inventory Variations 48
 Customer Invoice (€100 + 9% tax): remove COGST 50    to    Goods Issued Not Invoiced 50
 Customer Shipping:  Goods Shipment to Customer: 58000 Inventory Variations 50   to    14000 Inventory 50
 Production Order: to remove, needs a specific topic (cfr p.65 URSA Odoo Accounting Book)  

Configuration
    * Accounts Receivable/Payable: defined on the partner (Accounting tab)
    * Deferred Tax Assets/Liabilities: defined on the tax used on the invoice line
    * Revenues/Expenses: defined by default on product's internal category; can be also set in product form (Accounting tab) as a replacement value.
    * Inventory Variations: to set as Stock Input/Output Account in product's internal category
    * Inventory: to set as Stock Valuation Account in product's internal category

. h:div:: valuation-chart doc-aside

Anglo-Saxon Accounting
----------------------
current tab
- error to fix: Supplier Invoice (PO $50, Invoice $40)    -> Supplier Invoice (PO $50, Invoice $50)    
- add "($100 + 9% tax)" to Customer Invoice item (like Continental Accounting)
- Manufacturing Overhead: to remove (related account as well: 52000 Manufacturing Overhead)

Configuration
    * Accounts Receivable/Payable: defined on the partner (Accounting tab)
    * Deferred Tax Assets/Liabilities: defined on the tax used on the invoice line
    * Revenues/Expenses: defined by default on product's internal category and can be set in product form (Accounting tab) as a specific replacement value
    * Goods Received Not Purchased: to set as Stock Input Account in product's internal category
    * Goods Issued Not Invoiced: to set as Stock Output Account in product's internal category
    * Inventory: to set as Stock Valuation Account in product's internal category
    * Price Difference: to set in product's internal category or in product form as a specific replacement value

.. h:div:: valuation-chart doc-aside

For a more in-depth case including manufacturing operations (cfr p.65 URSA Odoo Accounting Book: production): LINK 
