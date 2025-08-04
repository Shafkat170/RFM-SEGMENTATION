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
Target potential high-value customers

Improve customer retention strategies




