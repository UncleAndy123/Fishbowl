# Fishbowl Open Sales Order Dashboard

A Power BI dashboard connected to Fishbowl Inventory (Advanced) via ODBC, giving a quick view of open sales orders with drill-through to line-item detail.

## Pages
<img width="1429" height="782" alt="Screenshot 2026-07-09 104427" src="https://github.com/user-attachments/assets/2ad6a8e0-3038-48a7-872b-04d1a13115f5" />
<img width="1911" height="1029" alt="Screenshot 2026-07-09 104446" src="https://github.com/user-attachments/assets/d5dd6c25-c936-4974-ac67-fcf0d162bff6" />
<img width="1049" height="799" alt="Screenshot 2026-07-09 105031" src="https://github.com/user-attachments/assets/e47b1011-fbec-4a6d-8210-2d7d400baaf3" />

### Sales Overview
- **Open Sales Orders** — card showing count of orders with `statusid` in an open state
- **Total Value of Open Sales Orders** — card showing sum of order totals
- **Count of id by name** — bar chart of open order count by customer
- **Order table** — SO number, customer name, total price, date created

### SO Detail (drill-through)
Right-click any row in the Sales Overview table → **Drill through → SO Detail** to open a filtered detail page for that order:
- Header cards: Sales Order number, Company Name, Date Created, Sales Order Value
- **SO Line Items** table: part number, description, unit price, quantity ordered, line note

Drill-through is configured with **Cross-report** and **Keep all filters** both on, filtered on `num`, `dateCreated`, `name`, and `totalPrice`.

## Data model

Tables used: `so`, `soitem`, `soitemstatus`, `sostatus`, `customer`, `part`, `product`.

Relationships follow Fishbowl's standard schema (`so.id → soitem.soid`, `customer.id → so.customerid`, `part.id → soitem.partid`). If you hit an "ambiguous paths" error when adding a relationship, it means two active paths already connect the same two tables — uncheck **Make this relationship active** on the new one to resolve it.

## Connecting to your own Fishbowl database

This report was built against a test database named `test_powerbi`, connected via an ODBC DSN of the same name.

**Easiest path — same DSN name:**
If you create an ODBC DSN on your machine also named `test_powerbi` pointing at your own Fishbowl MySQL instance, the report will connect with no changes required. Just open the `.pbix` and hit **Refresh**.

**Using a different DSN name:**
1. Open the report in Power BI Desktop.
2. Go to **Home → Transform data → Data source settings**.
3. Select the current source, click **Change Source**.
4. Pick your own ODBC DSN from the list (or enter a new connection string).
5. Click **OK**, then **Close & Apply**.
6. Re-enter database credentials if prompted, and click **Refresh**.

**Setting up the ODBC DSN itself (if you don't have one yet):**
1. Open **ODBC Data Sources (64-bit)** on Windows.
2. Add a new **System DSN** using a MySQL ODBC driver (e.g. MySQL ODBC 8.0 Unicode Driver).
3. Point it at your Fishbowl MySQL server, port, and the Fishbowl database name.
4. Test the connection, then save.
5. Use that DSN name in step 4 above.

## Notes

- Table and column names in this report use Fishbowl's native MySQL schema. If your Fishbowl version differs, some field names (e.g. `totalPrice` vs `subTotal`) may need remapping in Power Query.
