
### 1. Création de tables
## Table vente en cluster:

```sql
CREATE TABLE vente (
  id_prod NUMBER(6) NOT NULL,
  id_client NUMBER NOT NULL,
  id_temps DATE NOT NULL,
  id_canal CHAR(1) NOT NULL,
  id_promo NUMBER(6) NOT NULL,
  sold_quantite NUMBER(3) NOT NULL,
  sold_montant NUMBER(10,2) NOT NULL
)
CLUSTERING BY LINEAR ORDER (id_prod, id_client);
```
![alt text](<Capture d’écran du 2025-04-09 11-26-58.png>)

## Table vente2 partitionnée:

```sql
CREATE TABLE vente2 (
  id_prod NUMBER(6),
  id_client NUMBER,
  id_temps DATE,
  id_canal CHAR(1),
  id_promo NUMBER(6),
  sold_quantite NUMBER(3),
  sold_montant NUMBER(10,2)
)
PARTITION BY RANGE (id_temps) (
  PARTITION sales_q1_2006 VALUES LESS THAN (TO_DATE('01-APR-2006','dd-MON-yyyy')),
  PARTITION sales_q2_2006 VALUES LESS THAN (TO_DATE('01-JUL-2006','dd-MON-yyyy')),
  PARTITION sales_q3_2006 VALUES LESS THAN (TO_DATE('01-OCT-2006','dd-MON-yyyy')),
  PARTITION sales_q4_2006 VALUES LESS THAN (TO_DATE('01-JAN-2007','dd-MON-yyyy'))
);
```


```sql

```


```sql

```


```sql

```