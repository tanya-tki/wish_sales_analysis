# Analysis of Wish's Summer Clothes Sales Performance

## Introduction
Welcome to my project, where we look closely at summer clothes products from Wish, a popular online shopping site. Wish is where you can find almost anything at a good price. The way Wish shows shoppers different products is interesting, and that's why we're looking at their data.

This project isn't just about Wish. I believe doing this project can help us learn a lot about online shopping in general especially in low-price focused websites. What we find out can help other online stores make better decisions and can teach us more about what people like to buy online.

For this project, I will use SQL BigQuery and Excel to organize the data and make sense of it, and Tableau to create visuals. This will help us see the patterns and trends in the data more clearly.

These are the steps I will be using in this data analysis project:
<br>**Data Analysis Process: ask, prepare, process, analyze, share, and act.**

## 1.	Ask

Our goal is to understand better what makes products on Wish do well. We're looking at things like how often products are bought, what ratings they get, and their prices. This helps us see what shoppers like and don't like.

**Key questions guiding this analysis:**
1.	Is there a link between how much people like a product (its ratings) and how well it sells?
2.	Whether prices and discounts affect how likely people are to buy something?
3.	Which color of products are the most popular?
4.	Do the seller's fame factor into top products?
5.	Does the number of tags (making a product more discoverable) factor into the success of a product?

These questions aim to provide a comprehensive understanding of what drives customer choices on Wish, offering insights that could be beneficial for e-commerce strategies.

## 2.   Prepare

### 2.1 About Dataset
The data we're using shows us a list of products that come up when you search for "summer" on Wish. The data was collected in early August 2020 and posted on the Kaggle website for education purposes. It includes information like how many people bought these products, their ratings, and prices, including discounts. This is just a small part of all the data Wish has, but it's still really useful.

### 2.2 Data Organization 
The data consists of 2 datasets. All the datasets are in .csv file format and include long and wide formats.
    •	computed_insight_success_of_active_sellers
    •	summer_products_with_rating_and_performance

### 2.3 Data Limitations & Integrity 
#### 2.3.1 	Outdated Data
The summer clothes sales data was collected in August 2018 making the datasets outdated for current trend analysis. 
#### 2.3.2 	The Sample Size
The summer_products_with_rating_and_performance.csv dataset might not represent the entire range of summer products available in the market. 
#### 2.3.3 	Accuracy and Reliability
There is no explicit indication of the source or method of data collection for these datasets, which raises questions about their accuracy and reliability. Any errors in data collection or recording would directly impact the analysis.

## 3.	Analyze

### 3.1 Import CSV Files into a SQL Database
### 3.2 Initial Data Overview
#### Column Names and Row Count: 
Begin by identifying the column names and the total number of rows in the dataset. This initial step is essential for understanding the scope and scale of the data.
