# RFM Segmentation Using SQL on Sales Data
📌 Project Overview
This project demonstrates how to perform RFM (Recency, Frequency, Monetary) segmentation using SQL on real-world sales transaction data. The objective is to classify customers based on their purchasing behavior to help businesses make data-driven marketing decisions.

# What is RFM?
RFM is a marketing analysis technique used to quantitatively rank and segment customers based on:

Recency (R): How recently a customer made a purchase.

Frequency (F): How often a customer makes a purchase.

Monetary (M): How much money a customer spends.

# Why RFM Segmentation?
RFM helps:

Identify loyal customers

Detect lost customers

# Creating the Database
```
CREATE DATABASE IF NOT EXISTS RFM_SALES;
USE RFM_SALES;
CREATE TABLE SALES_SAMPLE_DATA (
    ORDERNUMBER INT(8),
    QUANTITYORDERED DECIMAL(8,2),
    PRICEEACH DECIMAL(8,2),
    ORDERLINENUMBER INT(3),
    SALES DECIMAL(8,2),
    ORDERDATE VARCHAR(16),
    STATUS VARCHAR(16),
    QTR_ID INT(1),
    MONTH_ID INT(2),
    YEAR_ID INT(4),
    PRODUCTLINE VARCHAR(32),
    MSRP INT(8),
    PRODUCTCODE VARCHAR(16),
    CUSTOMERNAME VARCHAR(64),
    PHONE VARCHAR(32),
    ADDRESSLINE1 VARCHAR(64),
    ADDRESSLINE2 VARCHAR(64),
    CITY VARCHAR(16),
    STATE VARCHAR(16),
    POSTALCODE VARCHAR(16),
    COUNTRY VARCHAR(24),
    TERRITORY VARCHAR(24),
    CONTACTLASTNAME VARCHAR(16),
    CONTACTFIRSTNAME VARCHAR(16),
    DEALSIZE VARCHAR(10)
);
```
# Inspecting Data
```
SELECT * FROM sample_sales_data LIMIT 10;
```
| ORDERNUMBER | QUANTITYORDERED | PRICEEACH | ORDERLINENUMBER | SALES   | ORDERDATE | STATUS  | QTR_ID | MONTH_ID | YEAR_ID | PRODUCTLINE | MSRP | PRODUCTCODE | CUSTOMERNAME            | PHONE         | ADDRESSLINE1                | ADDRESSLINE2 | CITY          | STATE | POSTALCODE | COUNTRY | TERRITORY | CONTACTLASTNAME | CONTACTFIRSTNAME | DEALSIZE |
|-------------|------------------|-----------|------------------|---------|------------|---------|--------|-----------|---------|--------------|------|--------------|--------------------------|---------------|------------------------------|---------------|---------------|--------|-------------|---------|-----------|------------------|------------------|----------|
| 10107       | 30               | 95.7      | 2                | 2871.00 | 24/2/03    | Shipped | 1      | 2         | 2003    | Motorcycles  | 95   | S10_1678     | Land of Toys Inc.        | 2125557818    | 897 Long Airport Avenue      |               | NYC           | NY     | 10022       | USA     | NA        | Yu               | Kwai             | Small    |
| 10121       | 34               | 81.35     | 5                | 2765.90 | 7/5/03     | Shipped | 2      | 5         | 2003    | Motorcycles  | 95   | S10_1678     | Reims Collectables       | 26.47.1555    | 59 rue de l’Abbaye           |               | Reims         |        | 51100       | France  | EMEA      | Henriot          | Paul             | Small    |
| 10134       | 41               | 94.74     | 2                | 3884.34 | 1/7/03     | Shipped | 3      | 7         | 2003    | Motorcycles  | 95   | S10_1678     | Lyon Souveniers          | +33 1 46 62…  | 27 rue du Colonel Pierre Avia|               | Paris         |        | 75508       | France  | EMEA      | Da Cunha         | Daniel           | Medium   |
| 10145       | 45               | 83.26     | 6                | 3746.70 | 25/8/03    | Shipped | 3      | 8         | 2003    | Motorcycles  | 95   | S10_1678     | Toys4GrownUps.com        | 6265557265    | 78934 Hillside Dr.           |               | Pasadena      | CA     | 90003       | USA     | NA        | Young            | Julie            | Medium   |
| 10159       | 49               | 100       | 14               | 5205.27 | 10/10/03   | Shipped | 4      | 10        | 2003    | Motorcycles  | 95   | S10_1678     | Corporate Gift Ideas Co. | 6505551386    | 7734 Strong St.              |               | San Francisco | CA     | 94103       | USA     | NA        | Brown            | Julie            | Medium   |
| ...         | ...              | ...       | ...              | ...     | ...        | ...     | ...    | ...       | ...     | ...          | ...  | ...          | ...                      | ...           | ...                          | ...           | ...           | ...    | ...         | ...     | ...       | ...              | ...              | ...      |

```
SELECT COUNT(*) FROM sample_sales_data;
```
---output---
| COUNT(*) |
|----------|
| 2823     |

-- Checking unique values
```
select distinct status from sample_sales_data;
```
| status      |
|-------------|
| Disputed    |
| In Process  |
| Cancelled   |
| On Hold     |
| Resolved    |

```
select distinct year_id from sample_sales_data;
```
| year_id |
|---------|
| 2003    |
| 2004    |
| 2005    |

```
select distinct PRODUCTLINE from sample_sales_data;
```
| PRODUCTLINE         |
|---------------------|
| Motorcycles         |
| Classic Cars        |
| Trucks and Buses    |
| Vintage Cars        |
| Planes              |
| Trains              |

```
select distinct COUNTRY from sample_sales_data;
```
| COUNTRY   |
|-----------|
| USA       |
| France    |
| Norway    |
| Australia |
| Finland   |
| ...       |

```
select distinct DEALSIZE from sample_sales_data;
```
| DEALSIZE |
|----------|
| Small    |
| Medium   |
| Large    |








