# Analysis of Wish's Summer Clothes Sales Performance
By Tanyaluck Kanai

## Introduction
Welcome to my project, where we look closely at summer clothes products from Wish, a popular online shopping site. Wish is where you can find almost anything at a good price. The way Wish shows shoppers different products is interesting, and that's why we're looking at their data.

This project isn't just about Wish. I believe doing this project can help us learn a lot about online shopping in general especially in low-price focused websites. What we find out can help other online stores make better decisions and can teach us more about what people like to buy online.

For this project, I will use SQL BigQuery and Excel to organize the data and make sense of it, and Tableau to create visuals. This will help us see the patterns and trends in the data more clearly.

The project follows these steps of data analysis process:
 <br> [1. Ask](/README.md#1ask)
  <br>[2. Prepare](/README.md#2---prepare)
  <br>[3. Process](/README.md#3process)
 <br> [4. Analyze](/README.md#2---analyze)
  <br>[5. Share](/README.md#2---prepare)
 <br> [6. Act](/README.md#2---prepare)

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
  *	computed_insight_success_of_active_sellers
  *	summer_products_with_rating_and_performance

### 2.3 Data Limitations & Integrity 
#### 2.3.1 	Outdated Data
The summer clothes sales data was collected in August 2018 making the datasets outdated for current trend analysis. 
#### 2.3.2 	The Sample Size
The summer_products_with_rating_and_performance.csv dataset might not represent the entire range of summer products available in the market. 
#### 2.3.3 	Accuracy and Reliability
There is no explicit indication of the source or method of data collection for these datasets, which raises questions about their accuracy and reliability. Any errors in data collection or recording would directly impact the analysis.

## 3.	Process

### 3.1 Import CSV Files into a SQL Database
### 3.2 Initial Data Overview
#### Column Names and Row Count: 
Begin by identifying the column names and the total number of rows in the dataset. This initial step is essential for understanding the scope and scale of the data.

```
SELECT column_name
FROM `project.wish_sales_datasets.INFORMATION_SCHEMA.COLUMNS`
WHERE table_name = ‘summer_products_with_rating_and_performance’
ORDER BY ordinal_position;
```

Belows are **summer_products_with_rating_and_performance** dataset column names:

column names = [‘title_orig, price, retail_price, currency_buyer, units_sold, uses_ad_boosts, rating, rating_count, rating_five_count, rating_four_count, rating_three_count, rating_two_count, rating_one_count, tags, product_color
product_variation_size_id, product_variation_inventory, shipping_option_name, shipping_option_price, shipping_is_express, countries_shipped_to, inventory_total, has_urgency_banner, urgency_text, origin_country, merchant_title, merchant_name, merchant_info_subtitle, merchant_rating_count, merchant_rating, merchant_id, merchant_has_profile_picture, merchant_profile_picture, product_id, theme, crawl_month’]

**summer_products_with_rating_and_performance dataset** has **36 columns** and **1,573 rows in total.**

#### Strategic Analysis Planning: 
Based on the dataset's structure and the information each column provides, I can plan out an analysis strategy. This will include deciding which columns are relevant to my questions and what kind of insights I can derive from them.

Below are some of the analysis ideas that I can think of after looking at the obtained column names and number of rows:

 1.	Customer Preference Insights:
*	Understand customer preferences based on product ratings (different rating counts) and the types of products sold (product_color, product_variation_size_id).
 2.	Price Analysis:
*	Evaluate how price affects units sold and ratings.
*	Study the effect of using ad boosts (uses_ad_boosts) on product sales and ratings.
 3.	Product Performance Analysis:
*	Examine which products perform best in terms of units sold (units_sold) and customer ratings (rating).

### 3.3 Let’s clean the data
3.3.1 Remove duplicate data by using the DISTINCT (*) and create new tables that have no duplication. We will use these new tables in further analysis.
* summer_products_with_rating_and_performance dataset has 34 duplicate values and has been removed; 1539 unique values remain. 
* product_id column found 198 duplicate values and has been removed; 1,341 unique values remain in total.

3.3.2	Check for ‘NULL’ value in all datasets.
```
SELECT 
COUNT(CASE WHEN title_orig IS NULL THEN 1 END) AS title_orig_nulls,
  	COUNT(CASE WHEN price IS NULL THEN 1 END) AS price_nulls,
 	 	.
.
.
  	COUNT(CASE WHEN theme IS NULL THEN 1 END) AS theme_nulls,
  	COUNT(CASE WHEN crawl_month IS NULL THEN 1 END) AS crawl_month_nulls
FROM project.wish_sales_datasets.summer_product_rating_and_performance
```
Note: If we use Python, the code will be much shorter. We can write a Python code as below:
```
null_counts_tablename = df_tablename.isnull().sum()
```
There are results I found from checking NULL values:
```
title_orig_nulls	    0
price_nulls	         0
retail_price_nulls	  0
currency_buyer_nulls	0
units_sold_nulls	    0
uses_ad_boosts_nulls	0
rating_nulls	        0
rating_count_nulls	  0
rating_five_count_nulls	   45
rating_four_count_nulls	   45
rating_three_count_nulls	  45
rating_two_count_nulls	    45
rating_one_count_nulls	    45
tags_nulls	          0
product_color_nulls	       41
product_variation_size_id_nulls	   13
product_variation_inventory_nulls	 0
shipping_option_name_nulls	  0
shipping_option_price_nulls	 0
shipping_is_express_nulls	   0
countries_shipped_to_nulls	  0
inventory_total_nulls       	0
has_urgency_banner_nulls	    1100
urgency_text_nulls	          1100
origin_country_nulls	        16
merchant_title_nulls	        0
merchant_name_nulls	         2
merchant_info_subtitle_nulls	1
merchant_rating_count_nulls	 0
merchant_rating_nulls	       0
merchant_id_nulls	           0
merchant_has_profile_picture_nulls	0
merchant_profile_picture_nulls	    1138
product_id_nulls	            0
theme_nulls	                 0
crawl_month_nulls	           0
```
What can we interpret from these data:
1.	Columns with No NULLS:
*	Many columns, such as title_orig, price, retail_price, and others, have zero null values. This suggests that these fields are consistently populated across dataset.
2.	Columns with NULLS:
*	Some products in columns product_color (41 nulls) and product_variation_size_id (14 nulls) might have no product variation. I will replace null value in product_color to no color variation.
*	The columns rating_five_count, rating_four_count, rating_three_count, rating_two_count, and rating_one_count each have 45 nulls. There might be products without any ratings, or these ratings are not captured consistently. I will replace the null values with zeros.
*	has_urgency_banner and urgency_text columns have 1100 nulls, indicating that most products in the dataset do not have an urgency banner. I will replace the null values with zeros.
*	merchant_profile_picture has 1138 nulls, suggesting that for many merchants, the profile picture data is missing.

3.3.3	Replaced NULL values with zero
Since the free trial version of Bigquery doesn’t allow SET and UPDATE functions which can modify the data directly, I had to find another way by creating a new table instead.
```
CREATE TABLE project.wish_sales_datasets.summer_product_rating_and_performance01 AS 
SELECT 
  title_orig,
  	.
.
  COALESCE(rating_five_count, 0) AS rating_five_count,
  COALESCE(rating_four_count, 0) AS rating_four_count,
  COALESCE(rating_three_count, 0) AS rating_three_count,
  COALESCE(rating_two_count, 0) AS rating_two_count,
  COALESCE(rating_one_count, 0) AS rating_one_count,
  	.
.
  COALESCE(has_urgency_banner, 0) AS has_urgency_banner,
  COALESCE(urgency_text, '0') AS urgency_text,
	.
	.  
  crawl_month  
FROM project.wish_sales_datasets.summer_product_rating_and_performance;
```
3.3.4	Checked for data inconsistency in product_color and product_variation_size_id
*	Run a query to list all distinct values in the product_color and product_variation_size_id columns. 
Since UPDATE function cannot be used in BigQuery. I decided to correct data inconsistency in Excel.
*	Convert all text to lowercase.
*	Map similar colors to a standard color name (e.g., "light blue" "navy blue" “lake blue” “sky blue” to "blue"). Map the same category size description into the same group (e.g., “SIZE S” to “s”)

4.	Analyze and Share
In this project, I've chosen to combine the steps of analysis and sharing. This is because I think it will be simpler for people to understand the insights and findings if they see the analysis process that led to them. By doing this, we can speed up decision-making in practice, as we share discoveries in real-time rather than waiting until all the analysis is complete.
4.1 Customer Preference Data Analysis
This data analysis focuses on the impact of product_color on units sold (units_sold) and customer ratings (rating) to gain customer preference Insights. These are the steps of product color analysis: 
4.1.1 Extracted all columns that will be used in product variation analysis by using SQL.
```
SELECT 
    product_color,
    SUM(units_sold) AS units_sold,
    AVG(rating) AS average_rating,
    COUNT(*) AS product_count
FROM 
    project.wish_sales_datasets.summer_products_with_rating_and_performance
GROUP BY 
    product_color
ORDER BY 
    units_sold DESC, average_rating DESC;
```
4.2.2 Created a graph by using Tableau to see if there are any correlations between product color, unit sold and average rating.


