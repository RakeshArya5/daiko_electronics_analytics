# Daiko Electronics — Global Manufacturing & Performance Analytics Dataset

## Company Background

**Daiko Electronics** is a fictional global electronics manufacturer headquartered in **Singapore**. The company designs and manufactures consumer electronics, industrial components, semiconductors, networking equipment, and power systems across a network of **12 factories in India, China, Vietnam, and Malaysia**.

Daiko sells to distributors, retailers, enterprise buyers, and government agencies across five regions: **Asia Pacific, Europe, North America, Middle East, and South Asia**. Its end-to-end operations — sourcing raw materials and components from 80 global suppliers, manufacturing across its factory network, holding inventory, fulfilling sales orders, and shipping via multiple logistics carriers — generate the operational data captured in this dataset.

## Files

| File | Rows | Description |
|---|---|---|
| `customers.csv` | 500 | B2B customer accounts (distributors, retailers, enterprises, government bodies) buying from Daiko |
| `products.csv` | 120 | Daiko's product catalog across five categories, with cost, price, and lifecycle status |
| `factories.csv` | 12 | Manufacturing plants across India, China, Vietnam, and Malaysia |
| `suppliers.csv` | 80 | Upstream suppliers of raw materials, components, packaging, chemicals, and logistics services |
| `purchase_orders.csv` | 3,000 | Inbound POs from suppliers to factories for materials/components |
| `sales_orders.csv` | 5,000 | Outbound customer sales orders fulfilled from factories |
| `inventory.csv` | 1,500 | Monthly stock snapshots per product/factory (Jan 2023–Dec 2024) |
| `logistics.csv` | 4,000 | Outbound shipments tied to sales orders, with carrier and delivery performance |
| `returns.csv` | 800 | Product returns tied to sales orders, with reason and resolution tracking |

## Column Reference

### customers.csv
| Column | Description |
|---|---|
| `customer_id` | Unique customer identifier (`CUST001`–`CUST500`) |
| `company_name` | B2B company name |
| `contact_name` | Primary contact, name style matched to country |
| `country` | Customer's country |
| `region` | Asia Pacific / Europe / North America / Middle East / South Asia |
| `city` | City within country |
| `customer_segment` | Distributor / Retailer / Enterprise / Government |
| `account_manager` | Assigned Daiko account manager (one of 15) |
| `credit_limit` | Approved credit limit (USD) |
| `payment_terms` | Net 30 / Net 45 / Net 60 / Net 90 |

### products.csv
| Column | Description |
|---|---|
| `product_id` | Unique product identifier (`PROD001`–`PROD120`) |
| `product_name` | Product model name |
| `category` | Consumer Electronics / Industrial Components / Semiconductors / Networking Equipment / Power Systems |
| `sub_category` | Category-specific sub-category |
| `unit_cost` | Manufacturing cost per unit (USD) |
| `unit_price` | List price per unit (USD) |
| `weight_kg` | Unit weight in kilograms |
| `warranty_months` | Warranty period (12 / 24 / 36) |
| `launch_date` | Product launch date |
| `status` | Active / Discontinued / Seasonal |

### factories.csv
| Column | Description |
|---|---|
| `factory_id` | Unique factory identifier (`FACT01`–`FACT12`) |
| `factory_name` | Plant name |
| `country` | India / China / Vietnam / Malaysia |
| `city` | Plant city |
| `region` | Company region grouping |
| `capacity_units_per_month` | Production capacity |
| `operational_since` | Date plant became operational |
| `specialization` | Product category the plant focuses on |
| `headcount` | Number of employees |
| `certification` | ISO certifications held (comma-separated) |

### suppliers.csv
| Column | Description |
|---|---|
| `supplier_id` | Unique supplier identifier (`SUPP001`–`SUPP080`) |
| `supplier_name` | Supplier company name |
| `country` | China / Japan / South Korea / India / Taiwan / Germany |
| `city` | Supplier city |
| `category` | Raw Materials / Electronic Components / Packaging / Chemicals / Logistics Services |
| `lead_time_days` | Typical order lead time |
| `reliability_score` | Historical on-time/quality reliability (0.60–0.99) |
| `payment_terms` | Net 30 / Net 45 / Net 60 |
| `contract_start` | Date contract began |
| `preferred_status` | Yes / No — preferred supplier flag |

### purchase_orders.csv
| Column | Description |
|---|---|
| `po_id` | Unique PO identifier (`PO00001`–`PO03000`) |
| `supplier_id` | FK → `suppliers.supplier_id` |
| `factory_id` | FK → `factories.factory_id` |
| `product_id` | FK → `products.product_id` |
| `order_date` | Date PO was placed |
| `expected_delivery_date` | `order_date` + supplier `lead_time_days` |
| `actual_delivery_date` | Actual delivery date (null for Pending/Cancelled) |
| `quantity_ordered` | Units ordered |
| `unit_cost` | Cost per unit on this PO |
| `total_cost` | `quantity_ordered` × `unit_cost` |
| `status` | Delivered / Pending / Delayed / Cancelled |
| `delay_days` | Days late (0 if on-time, null if Pending/Cancelled) |

### sales_orders.csv
| Column | Description |
|---|---|
| `order_id` | Unique order identifier (`ORD00001`–`ORD05000`) |
| `customer_id` | FK → `customers.customer_id` |
| `product_id` | FK → `products.product_id` (Active/Seasonal only) |
| `factory_id` | FK → `factories.factory_id` (fulfilling plant) |
| `order_date` | Date order was placed |
| `ship_date` | `order_date` + 2–14 days |
| `quantity` | Units ordered |
| `unit_price` | Price per unit on this order |
| `discount_pct` | Discount applied (%) |
| `revenue` | `quantity` × `unit_price` × (1 − `discount_pct`) |
| `region` | Matches the customer's `region` |
| `sales_channel` | Direct / Distributor / Online B2B / Government Tender |
| `priority` | High / Medium / Low |

### inventory.csv
| Column | Description |
|---|---|
| `inventory_id` | Unique snapshot row identifier (`INV00001`–`INV01500`) |
| `product_id` | FK → `products.product_id` |
| `factory_id` | FK → `factories.factory_id` |
| `snapshot_date` | Monthly snapshot date (Jan 2023–Dec 2024) |
| `quantity_on_hand` | Total units in stock |
| `quantity_reserved` | Units already allocated to orders |
| `quantity_available` | `quantity_on_hand` − `quantity_reserved` |
| `reorder_point` | Stock threshold that triggers replenishment |
| `reorder_quantity` | Units ordered when reorder point is hit |
| `days_of_stock` | Estimated days of supply remaining |
| `warehouse_zone` | A / B / C / D |

### logistics.csv
| Column | Description |
|---|---|
| `shipment_id` | Unique shipment identifier (`SHIP00001`–`SHIP04000`) |
| `order_id` | FK → `sales_orders.order_id` |
| `carrier` | DHL / FedEx / Maersk / DB Schenker / Nippon Express / Blue Dart |
| `ship_mode` | Air / Sea / Road / Rail |
| `origin_country` | Factory country (fulfilling plant) |
| `destination_country` | Customer's country |
| `dispatch_date` | Matches linked order's `ship_date` |
| `estimated_arrival` | Estimated arrival date based on `ship_mode` |
| `actual_arrival` | Actual arrival date (null while In Transit / Lost) |
| `freight_cost` | Shipping cost (USD) |
| `weight_kg` | Shipment weight (product weight × quantity) |
| `delivery_status` | Delivered / In Transit / Delayed / Lost |
| `delay_days` | Days late (0 if on-time, null if In Transit/Lost) |

### returns.csv
| Column | Description |
|---|---|
| `return_id` | Unique return identifier (`RET00001`–`RET00800`) |
| `order_id` | FK → `sales_orders.order_id` |
| `customer_id` | Matches linked order's `customer_id` |
| `product_id` | Matches linked order's `product_id` |
| `return_date` | 7–90 days after order's `ship_date` |
| `reason` | Defective / Damaged in Transit / Wrong Item Shipped / Customer Changed Mind / Warranty Claim |
| `quantity_returned` | Units returned |
| `refund_amount` | Refund value (USD) |
| `return_status` | Refunded / Pending / Rejected / Replaced |
| `resolution_days` | Days taken to resolve the return |
| `region` | Matches the customer's `region` |

## Entity Relationship Diagram

```
suppliers.csv                factories.csv                products.csv
   supplier_id ─┐                factory_id ─┐                product_id ─┐
                │                             │                            │
                ▼                             ▼                            ▼
                └──────────────► purchase_orders.csv ◄──────────────────┘
                                  (po_id, supplier_id, factory_id, product_id)


customers.csv                factories.csv                products.csv
   customer_id ─┐               factory_id ─┐                product_id ─┐
                │                            │                            │
                ▼                            ▼                            ▼
                └─────────────► sales_orders.csv ◄────────────────────┘
                                 (order_id, customer_id, factory_id, product_id)
                                          │           │
                    ┌─────────────────────┘           └───────────────────┐
                    ▼                                                     ▼
              logistics.csv                                        returns.csv
        (shipment_id, order_id)                              (return_id, order_id,
                                                             customer_id, product_id)

factories.csv ──┐
                ├──► inventory.csv (inventory_id, product_id, factory_id)
products.csv ───┘
```

**Key relationships:**
- `purchase_orders.supplier_id` → `suppliers.supplier_id`
- `purchase_orders.factory_id` / `sales_orders.factory_id` / `inventory.factory_id` → `factories.factory_id`
- `purchase_orders.product_id` / `sales_orders.product_id` / `inventory.product_id` → `products.product_id`
- `sales_orders.customer_id` → `customers.customer_id`
- `logistics.order_id` / `returns.order_id` → `sales_orders.order_id`
- `returns.customer_id` and `returns.product_id` are inherited from the linked `sales_orders` row (guaranteed consistent)
- `sales_orders.region` matches the linked customer's `region`; `logistics.destination_country` matches the linked customer's `country`

## Loading the Data

```python
import pandas as pd

# Replace with your repo's raw GitHub base URL after pushing, e.g.:
# BASE_URL = "https://raw.githubusercontent.com/<your-username>/<your-repo>/main/"
BASE_URL = "<RAW_GITHUB_BASE_URL_PLACEHOLDER>"

customers = pd.read_csv(BASE_URL + "customers.csv")
products = pd.read_csv(BASE_URL + "products.csv")
factories = pd.read_csv(BASE_URL + "factories.csv")
suppliers = pd.read_csv(BASE_URL + "suppliers.csv")
purchase_orders = pd.read_csv(BASE_URL + "purchase_orders.csv")
sales_orders = pd.read_csv(BASE_URL + "sales_orders.csv")
inventory = pd.read_csv(BASE_URL + "inventory.csv")
logistics = pd.read_csv(BASE_URL + "logistics.csv")
returns = pd.read_csv(BASE_URL + "returns.csv")
```

## Business Objectives Supported 
Here are five business objectives. Find exact analyticas quesiton in the projet document.

1. **Supply chain reliability analysis** — evaluate supplier on-time delivery performance (`purchase_orders` + `suppliers.reliability_score`) to identify sourcing risk and inform supplier consolidation or diversification decisions.
2. **Sales performance & customer segmentation** — analyze revenue, discounting, and channel mix across `sales_orders` and `customers` to identify top-performing segments, regions, and account managers.
3. **Inventory optimization** — use `inventory` snapshots against `sales_orders` demand to flag stockout/overstock risk, tune reorder points, and evaluate factory-level stock efficiency.
4. **Logistics & fulfillment performance** — assess carrier and shipping-mode reliability, freight cost efficiency, and delivery delays in `logistics` to optimize routing decisions across the factory network.
5. **Quality & returns management** — analyze `returns` reasons, resolution times, and refund exposure by product and region to identify quality issues and their downstream financial impact.
