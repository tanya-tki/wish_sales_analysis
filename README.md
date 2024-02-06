# Analysis of Wish's Summer Clothes Sales Performance

## Introduction
Welcome to my project, where we look closely at summer clothes products from Wish, a popular online shopping site. Wish is where you can find almost anything at a good price. The way Wish shows shoppers different products is interesting, and that's why we're looking at their data.

This project isn't just about Wish. I believe doing this project can help us learn a lot about online shopping in general especially in low-price focused websites. What we find out can help other online stores make better decisions and can teach us more about what people like to buy online.

For this project, I will use SQL BigQuery and Excel to organize the data and make sense of it, and Tableau to create visuals. This will help us see the patterns and trends in the data more clearly.

The case study follows these steps of data analysis process:

### [1. Ask]
### [2. Prepare]
### [3. Process]
### [4. Analyze]
### [5. Share]
### [6. Act]

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

Belows are **summer_products_with_rating_and_performance** dataset column names:

column names = [‘title_orig, price, retail_price, currency_buyer, units_sold, uses_ad_boosts, rating, rating_count, rating_five_count, rating_four_count, rating_three_count, rating_two_count, rating_one_count, tags, product_color
product_variation_size_id, product_variation_inventory, shipping_option_name, shipping_option_price, shipping_is_express, countries_shipped_to, inventory_total, has_urgency_banner, urgency_text, origin_country, merchant_title, merchant_name, merchant_info_subtitle, merchant_rating_count, merchant_rating, merchant_id, merchant_has_profile_picture, merchant_profile_picture, product_id, theme, crawl_month’]

**summer_products_with_rating_and_performance dataset** has **36 columns** and **1,573 rows in total.**

#### Strategic Analysis Planning: 
Based on the dataset's structure and the information each column provides, I can plan out an analysis strategy. This will include deciding which columns are relevant to my questions and what kind of insights I can derive from them.

Below are some of the analysis ideas that I can think of after looking at the obtained column names and number of rows:

 1.	Customer Preference Insights:
<br>o	Understand customer preferences based on product ratings (different rating counts) and the types of products sold (product_color, product_variation_size_id).
 2.	Price Analysis:
<br>o	Evaluate how price affects units sold and ratings.
<br>o	Study the effect of using ad boosts (uses_ad_boosts) on product sales and ratings.
 3.	Product Performance Analysis:
<br>o	Examine which products perform best in terms of units sold (units_sold) and customer ratings (rating).

### 3.3 Let’s clean the data
3.3.1 Remove duplicate data by using the DISTINCT (*) and create new tables that have no duplication. We will use these new tables in further analysis.

```
git status
git add
git commit
```

- summer_products_with_rating_and_performance dataset has 34 duplicate values and has been removed; 1539 unique values remain. 
- product_id column found 198 duplicate values and has been removed; 1,341 unique values remain in total.

3.3.2	Check for ‘NULL’ value in all datasets by using this code.
