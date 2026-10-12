# Bike Store Data Analysis (SQL + Python)

Analysis of a three-store bicycle retailer's sales, customers, products, inventory and staff, covering 2016–2018.
The data is loaded into MySQL, queried with SQL, and charted in a Jupyter notebook.
The goal is to answer common business questions: what sells, where, and who sells it.
 
## Project Files

| File | Description |
|---|---|
| `Bike_Store_Database.sql` | Creates the `Bike_Store` database and 9 tables, then loads the CSVs with `LOAD DATA INFILE`. |
| `Bike_Store_Analysis.sql` | 18 analysis queries in 6 groups: sales, customers, products, inventory, staff/stores, retention. |
| `Bike_Store_Analysis_Visualization.ipynb` | Runs the same questions from Python and plots the results (seaborn/matplotlib). |
| `Bike_Stores_Datasets/` | 9 source CSV files, one per table. |

## Dataset

| Table | Rows | Key fields |
|---|---|---|
| `orders` | 1,615 | order_id, customer_id, order_status (1–4), order_date, required_date, shipped_date, store_id, staff_id |
| `order_items` | 4,722 | order_id, item_id, product_id, quantity, list_price, discount |
| `customers` | 1,445 | customer_id, name, phone, email, street, city, state, zip_code |
| `products` | 321 | product_id, product_name, brand_id, category_id, model_year, list_price |
| `stocks` | 939 | store_id, product_id, quantity |
| `staffs` | 10 | staff_id, name, email, phone, active, store_id, manager_id |
| `stores` | 3 | store_id, store_name, contact fields, city, state (CA, NY, TX) |
| `brands` / `categories` | 9 / 7 | id and name lookup tables |

## Approach

1. **Understand the data:** profile 9 related tables (orders → items → products → brands/categories, plus stores, staff, stocks).
2. **Clean and load:** define the schema, then bulk-load the CSVs into MySQL.
3. **Analyse:** answer 6 groups of business questions with joins, aggregations, CTEs and window functions.
4. **Report:** re-run the queries in Python (`mysql.connector` → pandas) and chart each result.

## Key Insights

All figures are recomputed from the CSVs. Revenue is net of the discount actually stored in `order_items` (see Data Notes).

| Area | Finding |
|---|---|
| Scale | 1,615 orders, 7,078 units, **$7.69M net** ($8.58M gross; discounts cost $0.89M, 10.4%). |
| Trend | Net revenue: 2016 $2.43M (635 orders), 2017 $3.45M (688), 2018 $1.81M (292, mostly Jan–Apr). |
| Stores | Baldwin (NY) leads: 1,093 orders, $5.22M (67.8%). Santa Cruz: 348 orders, $1.61M. Rowlett: 174 orders, $0.87M. |
| Brands | Trek earns $4.60M (59.9%). Electra sells the most units (2,612) but earns $1.21M. |
| Categories | Cruisers sell the most units (2,063). Mountain Bikes earn the most ($2.72M). Electric Bikes sell the fewest (315). |
| Products | Most units: Surly Ice Cream Truck Frameset - 2016 (167). Top revenue: Trek Slash 8 27.5 - 2016 ($556K). |
| Customers | 90.9% ordered once (1,314); 92 ordered twice, 39 three times. 20 customers ordered 2+ times in 2018. |
| Geography | NY 1,093 orders, CA 348, TX 174. Top cities: Mount Vernon and Scarsdale (20 each). |
| Staff | Marcelene Boyer ($2.62M) and Venita Daniel ($2.59M), both Baldwin, drive 67.8% of revenue. |
| Fulfilment | 89.5% of orders are completed (status 4). 170 orders have no shipped date. |
| Inventory | 13,511 units in stock. 97 store–product rows hold fewer than 3 units, and 25 are at zero. |

## Tech Stack

- **Database:** MySQL 8.0, SQL (joins, aggregates, CTEs, `ROW_NUMBER()` window functions)
- **Python 3.12:** pandas, NumPy, matplotlib, seaborn, `mysql-connector-python`
- **Environment:** Jupyter Notebook

## How to Use

1. Copy `Bike_Stores_Datasets/*.csv` to your MySQL `secure_file_priv` folder, and update the paths in `Bike_Store_Database.sql`.
2. Run `Bike_Store_Database.sql`, then `Bike_Store_Analysis.sql`, in MySQL Workbench or the `mysql` client.
3. To rerun the charts: `pip install pandas numpy matplotlib seaborn mysql-connector-python jupyter`.
4. Set your own connection details in the first notebook cell, then run all cells. To only view results, open the notebook; the charts are saved in it.
