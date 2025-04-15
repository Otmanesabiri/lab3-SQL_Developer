#### Partie 3 : SQL (Objets avancés)

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
![alt text](<Capture d’écran du 2025-04-15 22-54-45.png>)

### 2. Gestion des séquences

## Création:

```sql
CREATE SEQUENCE id_prod
START WITH 1000
INCREMENT BY 1
NOCACHE
NOCYCLE;
```
![alt text](<Capture d’écran du 2025-04-15 22-59-41.png>)

## Suppression:

```sql
DROP SEQUENCE id_prod;
```
![alt text](<Capture d’écran du 2025-04-15 23-01-18.png>)

### 3. Dimensions et synonymes

## Création de dimension:

```sql
CREATE DIMENSION produits_dim
LEVEL canal IS (vente.id_canal)
LEVEL souscategorie IS (vente.id_client)
LEVEL categorie IS (vente.id_prod)
HIERARCHY prod_rollup (
  canal CHILD OF
  souscategorie CHILD OF
  categorie
)
ATTRIBUTE canal DETERMINES (vente.id_temps)
ATTRIBUTE souscategorie DETERMINES (vente.sold_montant)
ATTRIBUTE categorie DETERMINES (vente.id_promo);
```
![alt text](<Capture d’écran du 2025-04-15 23-05-48.png>)

## Création de synonyme:


```sql
CREATE SYNONYM commerce FOR vente;
```
![alt text](image.png)

#### Partie 4 : SQL Developer

### 1. Création des tables

## Table APPT:

```sql
CREATE TABLE APPT (
  NAppt INT NOT NULL,
  Ligneadresse1 VARCHAR(50),
  Ligneadresse2 VARCHAR(50),
  Ligneadresse3 VARCHAR(50),
  CodePostal INT,
  Ville VARCHAR(20),
  Pays VARCHAR(20),
  Batiment VARCHAR(20),
  Escalier VARCHAR(20),
  Etage INT,
  Porte INT,
  CONSTRAINT APPT_pk PRIMARY KEY (NAppt)
);
```
![alt text](<Capture d’écran du 2025-04-15 23-19-09.png>)

## Table ETREHUMAIN:
```sql
CREATE TABLE ETREHUMAIN (
  NUMSEC INT NOT NULL,
  NAppt INT,
  NOM VARCHAR(20),
  PRENOM VARCHAR(20),
  DATENAISSANCE DATE,
  SEXE VARCHAR(20),
  CONSTRAINT ETREHUMAIN_pk PRIMARY KEY (NUMSEC),
  CONSTRAINT ETREHUMAIN_APPT_fk FOREIGN KEY (NAppt) REFERENCES APPT(NAppt)
);
```
![alt text](<Capture d’écran du 2025-04-15 23-21-08.png>)

### 2. Gestion des utilisateurs
## Création utilisateur TP2:

```sql
ALTER SESSION SET "_ORACLE_SCRIPT"=TRUE;
CREATE USER TP2 IDENTIFIED BY TP2;
GRANT ALL ON APPT TO TP2;
GRANT ALL ON ETREHUMAIN TO TP2;
GRANT CONNECT TO TP2;
```
![alt text](<Capture d’écran du 2025-04-15 23-24-50.png>)

### 3. Insertion de données
## Exemple pour APPT:

```sql
INSERT INTO APPT (
  NAppt, 
  Ligneadresse1, 
  Ligneadresse2, 
  Ligneadresse3, 
  CodePostal, 
  Ville, 
  Pays, 
  Batiment, 
  Escalier, 
  Etage, 
  Porte
) 
VALUES (
  1, 
  'Appt1', 
  'Rue11', 
  '', 
  20000,  -- Example postal code (you need to provide actual value)
  'Casablanca', 
  'Maroc', 
  'A', 
  '2', 
  1, 
  101    -- Example door number (you need to provide actual value)
);
```
![alt text](<Capture d’écran du 2025-04-15 23-29-10.png>)



```sql

```


```sql

```


```sql

```


