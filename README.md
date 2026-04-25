# Datawave SQL Federation on Azure (Trino + Postgres + MySQL + Metabase)

This project demonstrates SQL federation using Trino as a query engine to join data across Postgres and MySQL, all running in Docker.
Metabase is used as the BI layer to run queries and visualize results.

------------------------------------------------------------

## What This Project Shows

✔ Trino connects to Postgres  
You can run:
SELECT * FROM postgres.public.orders

✔ Trino connects to MySQL  
You can run:
SELECT * FROM mysql.shipments.tracking

✔ Trino performs true SQL federation  
A single query joining Postgres + MySQL works:

SELECT 
    o.order_id,
    t.shipment_status
FROM postgres.public.orders o
JOIN mysql.shipments.tracking t
    ON o.order_id = t.order_id

This proves that federation is fully functional.

------------------------------------------------------------

## Project Structure

datawave-sql-federation-azure/
│
├── docker-compose.yml
├── trino/
│   └── etc/
│       ├── catalog/
│       │   ├── mysql.properties
│       │   └── postgres.properties
│       ├── config.properties
│       ├── jvm.config
│       └── node.properties
│
├── docker/
│   └── init-db/
│       ├── mysql.sql
│       └── postgres.sql
│
├── docs/
├── diagrams/
└── scripts/

------------------------------------------------------------

## How to Run

Start all services:

docker compose up -d

Services included:
- Trino (federated SQL engine)
- Postgres (orders dataset)
- MySQL (shipments dataset)
- Metabase (BI interface)

Once running:
- Trino UI → http://localhost:8080
- Metabase → http://localhost:3000

------------------------------------------------------------

## Testing Federation

1. Open Metabase → SQL Editor  
2. Run:

SHOW SCHEMAS FROM mysql
SHOW TABLES FROM mysql.shipments

3. Run the federated join:

SELECT 
    o.order_id,
    t.shipment_status
FROM postgres.public.orders o
JOIN mysql.shipments.tracking t
    ON o.order_id = t.order_id

If you see results, federation is confirmed.

------------------------------------------------------------

## Tools Used

- Trino — SQL federation engine  
- Postgres — relational database  
- MySQL — relational database  
- Docker Compose — container orchestration  
- Metabase — analytics + visualization  

------------------------------------------------------------

## Summary

This project demonstrates how Trino can unify multiple databases under a single SQL layer.
By connecting Postgres and MySQL through Trino and validating with a cross‑database join, the project proves that federated analytics is working end‑to‑end.
