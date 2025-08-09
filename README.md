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
---output---
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
---output---
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
---output---
| year_id |
|---------|
| 2003    |
| 2004    |
| 2005    |

```
select distinct PRODUCTLINE from sample_sales_data;
```
---output---
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
---output---
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
---output---
| DEALSIZE |
|----------|
| Small    |
| Medium   |
| Large    |

```
select distinct TERRITORY from sample_sales_data;
```
---output---
| TERRITORY |
|-----------|
| NA        |
| EMEA      |
| APAC      |
| Japan     |

```
SELECT DISTINCT
    MONTH_ID
FROM
    sample_sales_data
WHERE
    year_id = 2005
ORDER BY 1;
```
### 📊 Sample Output Table

| MONTH_ID |
|----------|
|    1     |
|    2     |
|    3     |
|    4     |
|    5     |

```
select year_id, round(sum(sales),2) as revenue
    from sample_sales_data
group by 1
ORDER BY 1;
```
| year_id |  revenue    |
|---------|-------------|
| 2003    | 3516979.54  |
| 2004    | 4724162.60  |
| 2005    | 1791486.71  |

```
SELECT MIN(str_to_date(ORDERDATE, '%d/%m/%y')) as first_business_day FROM SAMPLE_SALES_DATA;
```
| first_business_day |
|--------------------|
| 2003-01-06         |

```
SELECT MAX(str_to_date(ORDERDATE, '%d/%m/%y'))as last_business_day FROM SAMPLE_SALES_DATA;
```
| last_business_day  |
|--------------------|
| 2005-05-31         |

#  SubQuery
```
SELECT * FROM
(SELECT 
	CUSTOMERNAME,
    DATEDIFF((SELECT MAX(str_to_date(ORDERDATE, '%d/%m/%y')) FROM SAMPLE_SALES_DATA), MAX(STR_TO_DATE(ORDERDATE, '%d/%m/%y'))) AS R_VALUE,
    COUNT(DISTINCT ORDERNUMBER) AS F_VALUE,
    ROUND(SUM(SALES),0) AS M_Value
FROM SAMPLE_SALES_DATA
GROUP BY CUSTOMERNAME) AS SUMMARY_TABLE
WHERE R_VALUE BETWEEN 50 AND 100;
```
| CUSTOMERNAME               | R_VALUE | F_VALUE | M_Value |
|----------------------------|---------|---------|---------|
| Alpha Cognac               | 64      | 3       | 70488   |
| Anna's Decorations, Ltd    | 83      | 4       | 153996  |
| Auto Canal Petit           | 54      | 3       | 93171   |
| Corporate Gift Ideas Co.   | 97      | 4       | 149882  |
| Dragon Souveniers, Ltd.    | 90      | 5       | 172990  |
| FunGiftIdeas.com           | 89      | 3       | 98924   |
| Lyon Souveniers            | 75      | 3       | 78570   |
| Mini Auto Werke            | 82      | 3       | 52264   |
| ...                        | ...     | ...     | ...     |

# Analysis
# Final Query
```
CREATE OR REPLACE VIEW RFM AS
WITH CUSTOMER_SUMMARY_TABLE AS
(SELECT 
	CUSTOMERNAME,
    DATEDIFF((SELECT MAX(str_to_date(ORDERDATE, '%d/%m/%y')) FROM SAMPLE_SALES_DATA), MAX(STR_TO_DATE(ORDERDATE, '%d/%m/%y'))) AS RECENCY_VALUE,
    COUNT(DISTINCT ORDERNUMBER) AS FREQUENCY_VALUE,
    ROUND(SUM(SALES),0) AS MONETARY_VALUE
FROM SAMPLE_SALES_DATA
GROUP BY CUSTOMERNAME),
```
```
RFM_SCORE AS
(SELECT 
	S.*,
    NTILE(5) OVER(ORDER BY RECENCY_VALUE DESC) AS R_SCORE,
    NTILE(5) OVER(ORDER BY FREQUENCY_VALUE ASC) AS F_SCORE,
    NTILE(5) OVER(ORDER BY MONETARY_VALUE ASC) AS M_SCORE
FROM CUSTOMER_SUMMARY_TABLE AS S),
```
```
RFM_COMBINATION_SCORE AS
(SELECT 
	R.*,
    (R_SCORE+F_SCORE+M_SCORE) AS TOTAL_RFM_SCORE,
    CONCAT_WS('', R_SCORE, F_SCORE, M_SCORE) AS RFM_SCORE_COMBINATION
FROM RFM_SCORE AS R)
```
```
SELECT
	RC.*,
    CASE
		WHEN RFM_SCORE_COMBINATION IN (455, 542, 544, 552, 553, 452, 545, 554, 555) THEN 'Champions'
        WHEN RFM_SCORE_COMBINATION IN (344, 345, 353, 354, 355, 443, 451, 342, 351, 352, 441, 442, 444, 445, 453, 454, 541, 543, 515, 551) THEN 'Loyal Customers'
        WHEN RFM_SCORE_COMBINATION IN (513, 413, 511, 411, 512, 341, 412, 343, 514) THEN 'Potential Loyalists'
        WHEN RFM_SCORE_COMBINATION IN (414, 415, 214, 211, 212, 213, 241, 251, 312, 314, 311, 313, 315, 243, 245, 252, 253, 255, 242, 244, 254) THEN 'Promising Customers'
        WHEN RFM_SCORE_COMBINATION IN (141, 142,143,144,151,152,155,145,153,154,215) THEN 'Needs Attention'
        WHEN RFM_SCORE_COMBINATION IN (113, 111, 112, 114, 115) THEN 'About to Sleep'
        ELSE 'OTHER'
	END AS CUSTOMER_SEGMENT
FROM RFM_COMBINATION_SCORE AS RC;
```
```
SELECT * FROM RFM;
```
| CUSTOMERNAME                          | RECENCY_VALUE | FREQUENCY_VALUE | MONETARY_VALUE | R_SCORE | F_SCORE | M_SCORE | TOTAL_RFM_SCORE | RFM_SCORE_COMBINATION | CUSTOMER_SEGMENT      |
|----------------------------------------|---------------|-----------------|----------------|---------|---------|---------|-----------------|-----------------------|-----------------------|
| Boards & Toys Co.                      | 112           | 2               | 9129           | 4       | 2       | 1       | 7               | 421                   | OTHER                 |
| Atelier graphique                      | 187           | 3               | 24180          | 3       | 3       | 1       | 7               | 331                   | OTHER                 |
| Auto-Moto Classics Inc.                | 179           | 3               | 26479          | 3       | 3       | 1       | 7               | 331                   | OTHER                 |
| Microscale Inc.                        | 209           | 2               | 33145          | 2       | 1       | 1       | 4               | 211                   | Promising Customers   |
| Royale Belge                            | 141           | 4               | 33440          | 4       | 5       | 1       | 10              | 451                   | Loyal Customers       |
| Bavarian Collectables Imports, Co.     | 258           | 1               | 34994          | 1       | 1       | 1       | 3               | 111                   | About to Sleep        |
| Double Decker Gift Stores, Ltd         | 495           | 2               | 36019          | 1       | 1       | 1       | 3               | 111                   | About to Sleep        |
| ...                                    | ...           | ...             | ...            | ...     | ...     | ...     | ...             | ...                   | ...                   |













