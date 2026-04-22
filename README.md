# 🗄️ SQL — Data Analytics Portfolio

> **Week 3 of the Leep Talent Data Technician Skills Bootcamp (Level 3)**  
> This section showcases the SQL and database design skills I developed during the third week of my bootcamp, covering relational database theory, Entity Relationship Diagrams, schema design, and a full range of SQL queries across multiple real-world business scenarios using MySQL Workbench.

---

## 📁 Contents

| Task | Topic | Skills |
|------|-------|--------|
| [Day 1 · Task 1 — Database Concepts](#-day-1--task-1--database-concepts--erd) | Keys, relationships, ERD | Primary/Foreign keys, 1:1, 1:M, M:M |
| [Day 1 · Task 2 — Relational vs Non-Relational](#-day-1--task-2--relational-vs-non-relational-databases) | Database type comparison | RDBMS, NoSQL, use cases |
| [Day 3 · Task 1 — JOIN Types](#-day-3--task-1--join-types-in-mysql) | All six SQL JOIN types | INNER, LEFT, RIGHT, FULL, CROSS, SELF |
| [Day 4 · Task 1 — Retail Database Design](#-day-4--task-1--retail-database-design-convenience-store) | Schema design & SQL implementation | CREATE TABLE, INSERT, FK constraints, ERD |
| [Day 4 · Task 2 — World Database Queries](#-day-4--task-2--world-database-queries-mysql-workbench) | 18 real-world SQL queries | SELECT, WHERE, GROUP BY, HAVING, subqueries |
| [Business Scenario — NorthStar Homeware](#-business-scenario--northstar-homeware-sales-analysis) | Multi-table sales analysis | JOINs, aggregations, GROUP BY, ORDER BY |
| [Extended Queries — Subqueries & Aliases](#-extended-queries--subqueries--aliases) | Advanced SQL patterns | Subqueries, IN, aliases, multi-table JOINs |

---

## 🔑 Day 1 · Task 1 — Database Concepts & ERD

**Dataset:** Conceptual (nail salon scenario)  
**Tool:** MySQL Workbench, draw.io (ERD)  
**File:** [`ERD_v1.drawio`](./ERD_v1.drawio)

### About the Task

This task required researching and explaining core relational database concepts, connecting each one to a real-world scenario. The nail salon was used throughout as a consistent business context to ground abstract concepts in something practical and tangible.

> **In my own words:** Understanding primary and foreign keys is the foundation of everything else in SQL. Before writing a single query, you need to know how data is structured and why tables are linked the way they are. This task built that mental model using a real-world business rather than abstract theory.

### Real-World Context

**Organisation type:** Any business managing customer records, bookings, and transactions (salon, gym, retail, healthcare)  
A well-designed relational database prevents duplicate records, maintains data integrity across tables, and makes querying both faster and more reliable. Poor key design leads to data anomalies — where deleting or updating one record breaks relationships elsewhere.

### Key Concepts Answered

| Concept | My Answer |
|---------|-----------|
| **What is a primary key?** | A field that uniquely identifies each record in a table — no two rows can share the same value. In a nail salon, each customer gets a unique `CustomerID` even if two clients share a name. |
| **How does a secondary key differ?** | A secondary key is used for indexing and searching but does not have to be unique. In a salon, `ServiceType` (manicure, pedicure) could be a secondary key — many customers share the same service, so it cannot be a primary key. |
| **How are primary and foreign keys related?** | A primary key in one table becomes a foreign key in another to link the two. `CustomerID` is the primary key in the `Customers` table and appears as a foreign key in the `Bookings` table, connecting each booking to the correct customer. |
| **One-to-one example** | A staff member and their system login — one employee has one account, one account belongs to one employee only. |
| **One-to-many example** | One customer can have many bookings over time. The `Customers` table has one record per client; the `Bookings` table can have many rows for the same customer. |
| **Many-to-many example** | Nail technicians and services. One technician can perform many services; each service can be carried out by many technicians. A linking table (e.g. `TechnicianServices`) is required to resolve this relationship. |

### ERD Diagram

> 📸 *Open `ERD_v1.drawio` in draw.io (or VS Code with the draw.io extension). Capture a full screenshot of the complete ERD showing all tables, field names (with primary keys underlined/bold and foreign keys in italics), and relationship lines with cardinality notation (1:1, 1:M) clearly visible. Export as PNG and save as `images/day1_task1_erd.png`.*

![Entity Relationship Diagram](./images/day1_task1_erd.png)

---

### Key Findings

- **Finding 1:** Most relationships in a real database follow one-to-many — one customer has many bookings, one supplier supplies many products. This is the core pattern of relational design.
- **Finding 2:** Many-to-many relationships cannot be directly implemented in SQL. They always require a linking (junction) table in between, which becomes a natural place to store additional attributes about the relationship itself (e.g. the date a technician was certified for a particular service).

**What this means for the business:** A correctly keyed database prevents duplicate customer records, ensures bookings always trace back to a real customer, and makes querying — such as "show all bookings for customer C111" — fast and unambiguous.

### Portfolio Value

This task demonstrates that I understand database design at a conceptual level — not just how to write queries, but why the structure behind them matters. Employers expect junior analysts to be able to read an ERD and understand what it means for how data is queried.

---

## 🗂️ Day 1 · Task 2 — Relational vs Non-Relational Databases

**Tool:** Research and written analysis

### What I Covered

| Question | My Answer |
|----------|-----------|
| **Relational vs non-relational** | A relational database stores data in structured tables with clear relationships, using rows and columns with keys. A nail salon could have separate tables for customers, bookings, and services, all linked with keys. A non-relational database is more flexible — it stores data in formats like documents or key-value pairs without a fixed structure. |
| **What data benefits from non-relational?** | Data that doesn't follow a fixed structure or changes often — customer reviews, photos of nail designs, social media comments. Each entry may look different and wouldn't fit neatly into a table. Non-relational is better for variety and high volume without enforcing a rigid schema. |

### Screenshot

> 📸 *In MySQL Workbench, open any table in the world database. Capture the table structure view (right-click → Table Inspector, or the Schema panel showing table columns) to illustrate what a relational table looks like. Save as `images/day1_task2_relational_structure.png`.*

![Relational Structure](./images/day1_task2_relational_structure.png)

---

## 🔀 Day 3 · Task 1 — JOIN Types in MySQL

**Tool:** MySQL Workbench  
**Dataset:** world_db (city, country, countrylanguage tables)

### About the Task

Researched, explained, and demonstrated all six JOIN types in MySQL, applying each to a real-world business scenario using the nail salon as context.

> **In my own words:** JOINs are the most important SQL concept for a data analyst. Almost every real business question involves more than one table, and choosing the wrong JOIN type means getting the wrong answer — or silently missing data.

### Real-World Context

**Organisation type:** Any business with data split across related tables (retail, HR, healthcare, logistics)  
Most business questions require data from multiple tables: "show all customers and their orders" or "list all products and their suppliers." JOINs are how SQL brings that data together in a single result set.

### JOIN Types Explained

| JOIN Type | What it Returns | Nail Salon Example |
|-----------|----------------|-------------------|
| **INNER JOIN** | Only rows where there is a match in both tables | Customers who have made at least one booking — customers with no bookings are excluded |
| **LEFT JOIN** | All rows from the left table, NULLs for unmatched right rows | All customers including those who have never booked — NULL in the booking columns |
| **RIGHT JOIN** | All rows from the right table, NULLs for unmatched left rows | All bookings, even if somehow not linked to a customer record |
| **FULL JOIN** | All rows from both tables, NULLs where there's no match | Every customer and every booking, whether matched or not — useful for finding gaps |
| **CROSS JOIN** | Every possible combination of rows from both tables | Every technician paired with every service — produces a full matrix of possibilities |
| **SELF JOIN** | A table joined to itself | An employee table where one employee is the manager of another — reveals staff hierarchy |

### SQL Examples

```sql
-- INNER JOIN: Only cities that have a matching country
SELECT city.Name AS CityName, country.Name AS CountryName
FROM city
INNER JOIN country ON city.CountryCode = country.Code;

-- LEFT OUTER JOIN: All cities, country name NULL if no match
SELECT city.ID AS CityID, city.Name AS CityName, country.Name AS CountryName
FROM city
LEFT OUTER JOIN country ON city.CountryCode = country.Code;
```

### Screenshots

**Screenshot 1 — INNER JOIN result**
> 📸 *In MySQL Workbench, run the INNER JOIN query above on world_db. Capture the result grid showing CityName and CountryName columns, with a visible row count at the bottom. The query text should be visible in the editor pane above the results.*

![INNER JOIN](./images/day3_task1_inner_join.png)

---

**Screenshot 2 — LEFT OUTER JOIN result (showing NULLs)**
> 📸 *Run the LEFT OUTER JOIN query. Capture the result grid — if any NULL values appear in the CountryName column, ensure they are visible. Show both the query and the results together. The row count should be visible.*

![LEFT JOIN](./images/day3_task1_left_join.png)

---

**Screenshot 3 — JOIN Types Reference Diagram**
> 📸 *The uploaded `iconflash.jpg` (Joins in MySQL diagram) can be used directly here — it shows all six join types as Venn diagrams. Save a copy as `images/day3_task1_join_types.png`.*

![MySQL JOIN Types](./images/day3_task1_join_types.png)

---

### Key Findings

- **Finding 1:** INNER JOIN is the most common type in practice — it returns only confirmed, matched data, which is what most business reports need. Using LEFT JOIN when INNER JOIN was intended is one of the most common data analysis errors, as it silently inflates row counts.
- **Finding 2:** CROSS JOINs grow exponentially — two tables with 100 rows each produce 10,000 result rows. They should only be used deliberately for specific use cases like generating all combinations.

### Portfolio Value

Understanding when to use each JOIN type is a core analyst skill. This task demonstrates I can not only write the correct syntax but explain the business implication of choosing the wrong JOIN — a distinction that separates someone who can follow tutorials from someone who can reason about data.

---

## 🏪 Day 4 · Task 1 — Retail Database Design (Convenience Store)

**Task:** Group essay designing a full database system for a small corner shop with a loyalty programme  
**Tool:** MySQL Workbench, draw.io

### Scenario

> *Imagine you have been hired by a small retail business that wants to streamline operations by creating a new database system to manage inventory, sales, and customer information. The business is a small corner shop that sells groceries and domestic products. They also have a loyalty programme.*

### About the Design

The database follows a **snowflake schema** — with a central `Sales` fact table connected to dimension tables (`Customers`, `Inventory`, `Suppliers`) and a sub-dimension table (`Loyalty`) branching off `Customers`. This differs from a star schema, where all tables would connect directly to the fact table.

> **In my own words:** Before writing any SQL, you have to understand what the business actually needs to store and who needs to use it. A loyalty programme adds complexity because it creates a one-to-one relationship (one customer, one loyalty card) alongside the one-to-many relationships everywhere else. Getting that design right means the SQL queries will be straightforward later.

### Real-World Context

**Organisation type:** Independent retail business (corner shop, newsagent, convenience store)  
A small retailer needs to track stock levels to avoid running out of popular items, record sales to understand what sells well, manage supplier relationships to reorder efficiently, and run a loyalty scheme to reward regular customers. Without a structured database, all of this happens in disconnected spreadsheets — leading to errors, stock-outs, and missed loyalty rewards.

### Database Schema

**Tables designed:**

| Table | Primary Key | Key Fields | Relationships |
|-------|-------------|-----------|---------------|
| `Customers` | `customer_id` | name, contact, email, membership_status, points_total | 1:M to Sales, 1:1 to Loyalty |
| `Suppliers` | `supplier_id` | supplier_name, contact_details | 1:M to Inventory |
| `Inventory` | `product_id` | product_name, category, price, stock_count, *supplier_id* | M:1 from Suppliers, 1:M to Sales |
| `Sales` | `transaction_id` | sale_date, quantity, *customer_id*, *product_id* | M:1 from Customers and Inventory |
| `Loyalty` | `membership_no` | points_balance, *customer_id* | 1:1 with Customers |

**Relationship types:**
- Customers → Sales: **One-to-many** (one customer can have many transactions)
- Inventory → Sales: **One-to-many** (one product can appear in many transactions)
- Suppliers → Inventory: **One-to-many** (one supplier provides many products)
- Customers → Loyalty: **One-to-one** (one customer has exactly one loyalty card)

### SQL Implementation

**Creating the Inventory table with a foreign key constraint:**

```sql
CREATE TABLE Inventory (
    product_id       INT PRIMARY KEY,
    product_name     VARCHAR(100) NOT NULL,
    product_category VARCHAR(50)  NOT NULL,
    price            DECIMAL(10,2) NOT NULL,
    stock_count      INT NOT NULL,
    supplier_id      INT NOT NULL,
    FOREIGN KEY (supplier_id) REFERENCES Suppliers(supplier_id)
);
```

**Inserting initial data:**

```sql
INSERT INTO Inventory (product_name, product_category, price, stock_count, supplier_id)
VALUES ('Shampoo', 'Hair Care', 5.99, 100, 1);
```

**Data validation example:**

```sql
-- Ensure email format contains @
CHECK (email LIKE '%@%.%')

-- Ensure price is always positive
CHECK (price > 0)
```

### Screenshot

**Screenshot 1 — ERD / Schema Design**
> 📸 *Open draw.io or MySQL Workbench's Schema view. Capture the full database schema showing all 5 tables (Customers, Suppliers, Inventory, Sales, Loyalty) with their fields listed, primary keys shown in bold/underlined, foreign keys in italics, and relationship lines connecting the tables. The snowflake structure (chains of relationships rather than all radiating from one centre) should be visible. Save as `images/day4_task1_convenience_store_erd.png`.*

![Convenience Store ERD](./images/day4_task1_convenience_store_erd.png)

---

**Screenshot 2 — CREATE TABLE statement in MySQL Workbench**
> 📸 *In MySQL Workbench, type the CREATE TABLE Inventory statement above into the query editor. Before running it, capture the editor showing the full syntax with highlighting. The FOREIGN KEY constraint line should be clearly visible.*

![CREATE TABLE Statement](./images/day4_task1_create_table.png)

---

### Key Findings

- **Finding 1:** The Customers → Loyalty relationship is one-to-one — only one customer holds one loyalty card. This means the loyalty table's `customer_id` foreign key must also have a UNIQUE constraint to enforce this in SQL, not just in the design.
- **Finding 2:** Storing the database locally on a LAN (rather than cloud) is appropriate for a small independent shop — the POS system, back-office PC, and stock tablet can all connect, keeping costs low and access fast without requiring internet connectivity.

### Portfolio Value

This task demonstrates that I can approach a database design problem the same way a professional would: starting from business requirements, mapping them to entities and relationships, choosing the right schema type, and implementing it in SQL with proper constraints — not just writing queries against an existing structure.

---

## 🌍 Day 4 · Task 2 — World Database Queries (MySQL Workbench)

**Dataset:** `world_db` — MySQL world database (city, country, countrylanguage tables)  
**Tool:** MySQL Workbench

### About the Dataset

The MySQL world database is a standard sample dataset containing three related tables: `city` (4,079 records of cities worldwide), `country` (239 countries with population, GDP, life expectancy, and other metrics), and `countrylanguage` (official and spoken languages per country). It is used extensively for SQL practice because it mirrors the kind of multi-table relational structure found in real business databases.

> **In my own words:** The world database is a great SQL learning dataset because it's large enough to make queries meaningful but structured enough that you can reason about the results. Every query I ran had a real business scenario attached to it — a travel agency, a health initiative, a census bureau — which made the exercise feel closer to actual analyst work than just running commands for the sake of it.

### Real-World Context

**Organisation type:** Any analytics team working with structured, multi-table data (government, research, travel, real estate)  
The skills practised here — filtering, sorting, aggregating, joining, and using subqueries — map directly to the day-to-day tasks of a data analyst. The scenarios provided with each query simulate the kinds of requests that come from stakeholders in real organisations.

---

### Query Gallery

Each query below shows the scenario, the SQL used, and a screenshot placeholder.

---

**Query 1 — Count Cities in USA**

*Scenario: Demographic analysis — establish the total number of US cities as a baseline.*

```sql
SELECT COUNT(*) AS CityCount
FROM city
WHERE CountryCode = 'USA';
```

> 📸 *Run this query in MySQL Workbench on world_db. Capture both the query in the editor and the single-value result showing the count. Save as `images/day4_q1_count_usa_cities.png`.*

![Count USA Cities](./images/day4_q1_count_usa_cities.png)

---

**Query 2 — Country with Highest Life Expectancy**

*Scenario: Global health initiative — identify the country to prioritise for healthcare resources.*

```sql
SELECT Name, LifeExpectancy
FROM country
ORDER BY LifeExpectancy DESC
LIMIT 1;
```

> 📸 *Capture the query and result showing the single country with the highest life expectancy value. Both the country name and the numeric value should be visible. Save as `images/day4_q2_highest_life_expectancy.png`.*

![Highest Life Expectancy](./images/day4_q2_highest_life_expectancy.png)

---

**Query 3 — Cities Containing 'New' (New Year Promotion)**

*Scenario: Travel agency compiling city names for New Year promotional materials.*

```sql
SELECT Name, CountryCode
FROM city
WHERE Name LIKE '%New%';
```

> 📸 *Capture the query and result grid showing all cities with "New" in their name. Multiple rows should be visible. Save as `images/day4_q3_cities_new.png`.*

![Cities with New](./images/day4_q3_cities_new.png)

---

**Query 4 — First 10 Cities by Population**

*Scenario: Concise overview report of the world's most populous cities.*

```sql
SELECT Name, Population
FROM city
ORDER BY Population DESC
LIMIT 10;
```

> 📸 *Capture exactly 10 rows in the result grid, sorted with the highest population at the top. Column headers (Name, Population) should be visible. Save as `images/day4_q4_top10_cities.png`.*

![Top 10 Cities by Population](./images/day4_q4_top10_cities.png)

---

**Query 5 — Cities with Population Over 2 Million**

*Scenario: Real estate developer screening cities for investment opportunities.*

```sql
SELECT Name, Population
FROM city
WHERE Population > 2000000
ORDER BY Population DESC;
```

> 📸 *Capture the result showing cities above 2M population. The highest-population cities (Tokyo, São Paulo, etc.) should appear at the top. Save as `images/day4_q5_population_over_2m.png`.*

![Cities Over 2M Population](./images/day4_q5_population_over_2m.png)

---

**Query 6 — Cities Beginning with 'Be'**

*Scenario: Travel blogger compiling cities with unique names starting with 'Be'.*

```sql
SELECT Name, CountryCode
FROM city
WHERE Name LIKE 'Be%';
```

> 📸 *Capture the result showing cities beginning with "Be" (e.g. Berlin, Belgrade, Beirut). Save as `images/day4_q6_cities_be.png`.*

![Cities Starting with Be](./images/day4_q6_cities_be.png)

---

**Query 7 — Cities with Population Between 500,000 and 1,000,000**

*Scenario: Urban planning committee identifying mid-sized cities for infrastructure projects.*

```sql
SELECT Name, Population
FROM city
WHERE Population BETWEEN 500000 AND 1000000
ORDER BY Population DESC;
```

> 📸 *Capture the result. Population values should visibly fall within the 500K–1M range. Save as `images/day4_q7_mid_cities.png`.*

![Mid-Sized Cities](./images/day4_q7_mid_cities.png)

---

**Query 8 — Cities Sorted Alphabetically (A–Z)**

*Scenario: Geography teacher preparing a lesson on alphabetical ordering of city names.*

```sql
SELECT Name, CountryCode
FROM city
ORDER BY Name ASC;
```

> 📸 *Capture the first page of results showing cities sorted alphabetically from A. Names beginning with 'A' should be visible at the top. If you applied text filtering in Workbench to improve readability, capture with that formatting visible. Save as `images/day4_q8_cities_alphabetical.png`.*

![Cities Alphabetical](./images/day4_q8_cities_alphabetical.png)

---

**Query 9 — Most Populated City**

*Scenario: Real estate investment firm identifying the single most populous city.*

```sql
SELECT Name, Population
FROM city
ORDER BY Population DESC
LIMIT 1;
```

> 📸 *Capture the single-row result showing the most populated city and its population figure. Save as `images/day4_q9_most_populated_city.png`.*

![Most Populated City](./images/day4_q9_most_populated_city.png)

---

**Query 10 — City Name Frequency Analysis**

*Scenario: Geography teacher needing unique city names and how many times each appears.*

```sql
SELECT Name, COUNT(*) AS Occurrences
FROM city
GROUP BY Name
ORDER BY Name ASC;
```

> 📸 *Capture the result showing city names and their occurrence counts, sorted alphabetically. Any city name that appears more than once (e.g. common names like "San Jose") should be visible with a count greater than 1. Save as `images/day4_q10_city_frequency.png`.*

![City Name Frequency](./images/day4_q10_city_frequency.png)

---

**Query 11 — City with the Lowest Population**

*Scenario: Census bureau mapping the full distribution of urban population.*

```sql
SELECT Name, Population
FROM city
ORDER BY Population ASC
LIMIT 1;
```

> 📸 *Capture the single-row result showing the least populated city. Save as `images/day4_q11_lowest_population_city.png`.*

![Lowest Population City](./images/day4_q11_lowest_population.png)

---

**Query 12 — Country with Largest Population**

*Scenario: Global economic research institute profiling the highest-population nations.*

```sql
SELECT Name, Population
FROM country
ORDER BY Population DESC
LIMIT 1;
```

> 📸 *Capture the single-row result. Save as `images/day4_q12_largest_country.png`.*

![Largest Country Population](./images/day4_q12_largest_country.png)

---

**Query 13 — Capital of Spain**

*Scenario: Travel agency verifying European capital cities for tour itineraries.*

```sql
SELECT ci.Name AS Capital
FROM country co
JOIN city ci ON co.Capital = ci.ID
WHERE co.Name = 'Spain';
```

> 📸 *Capture the single-row result showing Madrid. Both the query (including the JOIN) and the result should be visible. Save as `images/day4_q13_capital_spain.png`.*

![Capital of Spain](./images/day4_q13_capital_spain.png)

---

**Query 14 — Cities in Europe**

*Scenario: European cultural exchange programme listing all continental cities.*

```sql
SELECT ci.Name AS City, co.Name AS Country
FROM city ci
JOIN country co ON ci.CountryCode = co.Code
WHERE co.Continent = 'Europe'
ORDER BY co.Name, ci.Name;
```

> 📸 *Capture a portion of the result showing cities and their European countries. The JOIN syntax and the WHERE filter on Continent should be visible in the editor. Save as `images/day4_q14_cities_europe.png`.*

![Cities in Europe](./images/day4_q14_cities_europe.png)

---

**Query 15 — Average City Population by Country**

*Scenario: Demographic research team comparing population distributions across nations.*

```sql
SELECT co.Name AS Country, AVG(ci.Population) AS AvgCityPopulation
FROM city ci
JOIN country co ON ci.CountryCode = co.Code
GROUP BY co.Name
ORDER BY AvgCityPopulation DESC;
```

> 📸 *Capture the result showing countries and their average city population figures, sorted descending. Save as `images/day4_q15_avg_population.png`.*

![Average City Population](./images/day4_q15_avg_population.png)

---

**Query 16 — Countries with Low Population Density**

*Scenario: Agricultural research institute identifying sparsely populated countries for development.*

```sql
SELECT Name, Population, SurfaceArea,
       ROUND(Population / SurfaceArea, 2) AS PopDensity
FROM country
WHERE SurfaceArea > 0
ORDER BY PopDensity ASC
LIMIT 20;
```

> 📸 *Capture the result showing the 20 least densely populated countries. The calculated PopDensity column should be visible. Save as `images/day4_q16_low_density.png`.*

![Low Population Density](./images/day4_q16_low_density.png)

---

**Query 17 — Countries with High GDP per Capita**

*Scenario: Economic consulting firm identifying high-income countries for investment.*

```sql
SELECT Name, GNP, Population,
       ROUND(GNP / Population * 1000, 2) AS GNP_per_Capita
FROM country
WHERE Population > 0
HAVING GNP_per_Capita > (SELECT AVG(GNP / Population * 1000) FROM country WHERE Population > 0)
ORDER BY GNP_per_Capita DESC;
```

> 📸 *Capture the result with the calculated GNP_per_Capita column visible. Countries like Luxembourg, Switzerland, and Norway should appear near the top. Save as `images/day4_q17_high_gdp.png`.*

![High GDP per Capita](./images/day4_q17_high_gdp.png)

---

**Query 18 — Cities Ranked 31–40 by Population (Pagination)**

*Scenario: Market research firm needing cities outside the top rankings for comprehensive analysis.*

```sql
SELECT Name, Population
FROM city
ORDER BY Population DESC
LIMIT 10 OFFSET 30;
```

> 📸 *Capture exactly 10 rows showing cities ranked 31st through 40th by population. The query with LIMIT and OFFSET visible in the editor and the 10-row result grid should both be captured. Save as `images/day4_q18_rows_31_40.png`.*

![Cities Ranked 31-40](./images/day4_q18_rows_31_40.png)

---

### Key Findings

- **Finding 1:** The `LIMIT` + `OFFSET` combination (Query 18) is the foundation of data pagination — the same technique used by websites showing search results across multiple pages. Understanding it demonstrates awareness of how SQL powers real applications, not just static reports.
- **Finding 2:** The `LIKE '%New%'` wildcard query (Query 3) returns any city containing "New" anywhere in the name — including at the start, middle, or end. This highlights the importance of understanding wildcard placement, as `'New%'` and `'%New'` would return different results.

### Portfolio Value

These 18 queries, each tied to a distinct business scenario, demonstrate fluency across the full range of basic-to-intermediate SQL — filtering, sorting, aggregating, joining, and paginating. This is the practical core of what a junior data analyst is expected to do from day one.

---

## 📊 Business Scenario — NorthStar Homeware Sales Analysis

**Company:** NorthStar Homeware — a UK home and kitchen products retailer operating across 3 regions (North, South, Midlands)  
**Tool:** Online SQL Editor (programiz.com)

### About the Dataset

Three custom tables were created for this scenario: `Regions` (5 regions with monthly sales targets), `Salespeople` (5 salespeople each assigned to a region), and `Sales` (individual transactions with product, quantity, value, and linked salesperson/region).

> **In my own words:** This scenario was the closest to a real analyst brief — a Managing Director concerned about inconsistent performance wanted specific answers about which regions and salespeople were hitting targets, and where stock or effort gaps might be causing lost sales. Writing queries that answer those questions directly, not just exploring data for its own sake, is what data analysis is actually for.

### Real-World Context

**Organisation type:** UK retailer with a regional sales force  
A company with multiple regions and salespeople needs to compare performance fairly, identify who needs support, and understand whether missed targets are due to people, products, or stock levels. SQL is the fastest way to surface these insights from transactional data.

### Tables Used

```sql
-- Regions: region_id, region_name, monthly_sales_target
-- Salespeople: salesperson_id, salesperson_name, region_id
-- Sales: sale_id, sale_date, region_id, salesperson_id, product_name, quantity_sold, sale_value
```

### 10 Analytical Queries

**Query 1 — All Regions and Their Sales Targets**

```sql
SELECT region_name, monthly_sales_target
FROM Regions;
```

> 📸 *Run in the online SQL editor. Capture the full result showing all 5 regions and their target values. Save as `images/biz_q1_regions_targets.png`.*

![Regions and Targets](./images/biz_q1_regions_targets.png)

---

**Query 2 — All Sales with Region, Salesperson, and Value (JOIN)**

```sql
SELECT s.sale_id, r.region_name, sp.salesperson_name, s.sale_value
FROM Sales s
JOIN Regions r ON s.region_id = r.region_id
JOIN Salespeople sp ON s.salesperson_id = sp.salesperson_id;
```

> 📸 *Capture the joined result table showing sale_id, region name, salesperson name, and sale value together in a single view. Save as `images/biz_q2_sales_joined.png`.*

![Sales Joined](./images/biz_q2_sales_joined.png)

---

**Query 3 — Total Sales Value by Region**

```sql
SELECT r.region_name, SUM(s.sale_value) AS total_sales
FROM Sales s
JOIN Regions r ON s.region_id = r.region_id
GROUP BY r.region_name;
```

> 📸 *Capture the aggregated result showing each region and its total. Save as `images/biz_q3_total_by_region.png`.*

![Total Sales by Region](./images/biz_q3_total_by_region.png)

---

**Query 4 — Total Sales vs Target by Region**

```sql
SELECT r.region_name, SUM(s.sale_value) AS total_sales, r.monthly_sales_target
FROM Regions r
JOIN Sales s ON r.region_id = s.region_id
GROUP BY r.region_name, r.monthly_sales_target;
```

> 📸 *Capture the result showing three columns: region, total_sales, and monthly_sales_target side-by-side. It should be immediately visible which regions hit or missed their target. Save as `images/biz_q4_vs_target.png`.*

![Sales vs Target](./images/biz_q4_vs_target.png)

---

**Query 5 — Total Sales by Salesperson**

```sql
SELECT sp.salesperson_name, SUM(s.sale_value) AS total_sales
FROM Sales s
JOIN Salespeople sp ON s.salesperson_id = sp.salesperson_id
GROUP BY sp.salesperson_name;
```

> 📸 *Save as `images/biz_q5_salesperson_totals.png`.*

![Salesperson Totals](./images/biz_q5_salesperson_totals.png)

---

**Query 6 — Top 3 Salespeople by Value**

```sql
SELECT sp.salesperson_name, SUM(s.sale_value) AS total_sales
FROM Sales s
JOIN Salespeople sp ON s.salesperson_id = sp.salesperson_id
GROUP BY sp.salesperson_name
ORDER BY total_sales DESC
LIMIT 3;
```

> 📸 *Capture exactly 3 rows, sorted highest-to-lowest. Save as `images/biz_q6_top3_salespeople.png`.*

![Top 3 Salespeople](./images/biz_q6_top3_salespeople.png)

---

**Query 7 — Region with Highest Total Sales**

```sql
SELECT r.region_name, SUM(s.sale_value) AS total_sales
FROM Sales s
JOIN Regions r ON s.region_id = r.region_id
GROUP BY r.region_name
ORDER BY total_sales DESC
LIMIT 1;
```

> 📸 *Single-row result showing the top region. Save as `images/biz_q7_top_region.png`.*

![Top Region](./images/biz_q7_top_region.png)

---

**Query 8 — Number of Transactions per Salesperson**

```sql
SELECT sp.salesperson_name, COUNT(*) AS number_of_sales
FROM Sales s
JOIN Salespeople sp ON s.salesperson_id = sp.salesperson_id
GROUP BY sp.salesperson_name;
```

> 📸 *Save as `images/biz_q8_transaction_count.png`.*

![Transaction Count](./images/biz_q8_transaction_count.png)

---

**Query 9 — Total Quantity Sold per Product**

```sql
SELECT product_name, SUM(quantity_sold) AS total_quantity
FROM Sales
GROUP BY product_name;
```

> 📸 *Save as `images/biz_q9_quantity_by_product.png`.*

![Quantity by Product](./images/biz_q9_quantity_by_product.png)

---

**Query 10 — North Region: Salesperson Performance**

```sql
SELECT sp.salesperson_name, SUM(s.sale_value) AS total_sales
FROM Sales s
JOIN Salespeople sp ON s.salesperson_id = sp.salesperson_id
JOIN Regions r ON s.region_id = r.region_id
WHERE r.region_name = 'North'
GROUP BY sp.salesperson_name;
```

> 📸 *Capture the filtered result showing only North region data. Save as `images/biz_q10_north_region.png`.*

![North Region Performance](./images/biz_q10_north_region.png)

---

### Analysis Report

After running all 10 queries, the key findings were:

- Amir (North, £2,400) achieved the highest individual sale value, making the North the top-performing region
- Lucy (South, £1,750) was the second-highest performer, but the South has the highest monthly target (£22,000) — indicating the region may need additional resource
- Jade (Midlands, £800) had the lowest total sales value, suggesting either limited opportunities in the region or a need for additional support
- Headphones had the highest quantity sold (10 units) despite a lower unit value, while Laptops generated the highest revenue per transaction
- **Recommendation 1:** Focus coaching or support on the Midlands region and review whether the monthly target is set appropriately for the market size
- **Recommendation 2:** Consider reallocating product inventory toward high-volume items in stronger regions to maximise revenue where demand is already established

---

## 🔍 Extended Queries — Subqueries & Aliases

**Dataset:** world_db (MySQL Workbench)  
**Tool:** MySQL Workbench

### What I Covered

These queries extend basic SQL with subqueries, aliases, and more complex filtering — moving from simple SELECT statements to queries that answer compound questions in a single statement.

### Key Queries

**Subquery — Languages spoken in countries with population over 100 million:**

```sql
SELECT Language
FROM countrylanguage
WHERE CountryCode IN (
    SELECT Code FROM country WHERE Population > 100000000
);
```

**Subquery — Cities in the most populated country:**

```sql
SELECT Name
FROM city
WHERE CountryCode = (
    SELECT Code FROM country ORDER BY Population DESC LIMIT 1
);
```

**Alias example — country name and population with readable column headers:**

```sql
SELECT c.Name AS Country, c.Population AS Population
FROM country c;
```

**JOIN with aliases — country name and spoken language:**

```sql
SELECT c.Name AS Country, cl.Language AS Language
FROM country c
JOIN countrylanguage cl ON c.Code = cl.CountryCode;
```

**Countries with population above world average (subquery in WHERE):**

```sql
SELECT Name, Population
FROM country
WHERE Population > (SELECT AVG(Population) FROM country)
ORDER BY Population DESC;
```

### Screenshots

**Screenshot 1 — Subquery: Languages in large countries**
> 📸 *Run the countrylanguage subquery. Capture the result showing the language list, and ensure the query with the IN (subquery) syntax is visible in the editor above. Save as `images/ext_q1_languages_subquery.png`.*

![Languages Subquery](./images/ext_q1_languages_subquery.png)

---

**Screenshot 2 — Cities in most populated country**
> 📸 *Run the city subquery. Capture the result — it should show cities in China ('CHN'). The nested query structure should be visible. Save as `images/ext_q2_cities_most_populated.png`.*

![Cities Most Populated Country](./images/ext_q2_cities_most_populated.png)

---

**Screenshot 3 — Countries above world average population**
> 📸 *Run the WHERE subquery. Capture the result showing countries ordered by population, all above the world average. Save as `images/ext_q3_above_avg_population.png`.*

![Above Average Population](./images/ext_q3_above_avg_population.png)

---

### Key Findings

- **Finding 1:** Subqueries make compound questions answerable in a single statement — "find cities in the most populated country" would otherwise require two separate queries and manual lookup. This dramatically reduces the steps between a business question and an answer.
- **Finding 2:** Aliases (`AS Country`, `c.Name`) dramatically improve query readability and are essential when joining tables that share column names like `Name` or `ID`. Without aliases, result columns become ambiguous and difficult to interpret.

### Portfolio Value

Subqueries and aliases are what separate intermediate SQL from beginner-level queries. This section demonstrates I can write self-contained queries that answer multi-step business questions without needing intermediate manual steps.

---

## 🛠️ Tools & Techniques Used

- **MySQL Workbench** — primary environment for running queries against world_db
- **Online SQL Editor** (programiz.com) — used for the NorthStar Homeware business scenario
- **draw.io** — Entity Relationship Diagrams
- **SQL commands:** `SELECT`, `WHERE`, `ORDER BY`, `LIMIT`, `OFFSET`, `GROUP BY`, `HAVING`, `COUNT`, `SUM`, `AVG`, `MAX`, `MIN`, `LIKE`, `BETWEEN`, `IN`, `DISTINCT`
- **JOIN types:** `INNER JOIN`, `LEFT OUTER JOIN`, `RIGHT JOIN`, `FULL JOIN`, `CROSS JOIN`, `SELF JOIN`
- **Advanced features:** Subqueries (`IN`, scalar subqueries), table and column aliases (`AS`), `CHECK` constraints, `FOREIGN KEY` references, `CREATE TABLE`, `INSERT INTO`

---

## 📂 Datasets & Files

| File | Description | Source |
|------|-------------|--------|
| `world_db` | MySQL standard sample database — cities, countries, languages | MySQL / Bootcamp |
| `ERD_v1.drawio` | Entity Relationship Diagram for nail salon database design | Bootcamp (Day 1 Task 1) |
| `student.csv` | Student marks dataset (used in Excel week, referenced here for context) | Bootcamp |
| NorthStar Homeware tables | Custom Regions, Salespeople, Sales tables created in SQL | Bootcamp (Business Scenario) |

---

*← [Back to Portfolio](https://github.com/chansg/chansg)*
