# Analysis of Wish's Summer Clothes Sales Performance

By: Tanyaluck Kanai
<br>Project Status: Completed
<br> Date: 13 Feb 2024

For a quick understanding of our analysis and to see the story our data tells, please visit our Tableau story here: Link to
[Wish Summer Clothings Sales Analysis Story Dashboard - Tableau](https://public.tableau.com/app/profile/tanyaluck.kanai/viz/WishSummerSalesAnalysis/WishSalesAnalysis?publish=yes)


<img src="https://github.com/tanya-tki/wish_sales_analysis/assets/153815515/fdc3edaf-a6f3-47be-822e-60b0323d46b1" width="400">



## Introduction
Welcome to my project, where we look closely at summer clothes products from Wish, a popular online shopping site. Wish is where you can find almost anything at a good price.

This project isn't just about Wish. I believe doing this project can help us learn a lot about online shopping in general especially in low-price focused e-commerce. What we find out can help other online stores make better decisions and can teach us more about what people like to buy online.

For this project, SQL BigQuery and Excel will be used to organize the data and make sense of it, and Tableau to create visuals. This will help us see the patterns and trends in the data more clearly.

One of the reason this project was developed is to showcase of my analytical and technical skills. It highlights my ability to derive actionable insights from complex datasets and propose strategic recommendations, reflecting my potential value to prospective employers in data analysis and business intelligence roles.

If you have any questions or recommendations, please do not hesitate to DM me.

## The project follows these steps of data analysis process:
 [1. Ask](/README.md#1ask)
  <br>[2. Prepare](/README.md#2---prepare)
  <br>[3. Process](/README.md#3process)
 <br> [4. Analyze & Share](/README.md#4analyze-and-share)
 <br> [5. Act](/README.md#5act)

## 1.	Ask

The objective of this project is to conduct a comprehensive analysis of sales data from the Wish platform in clothing category to identify opportunities for marketing strategies. By leveraging data analytics techniques, we aim to gain actionable insights into customer behavior, product performance and pricing strategies.

**Key questions guiding this analysis:**
1. Is there a link between how much people like a product (its ratings) and how well it sells?
2. Whether prices and discounts affect how likely people are to buy something?
3. Which color of products are the most popular?

These questions aim to provide a comprehensive understanding of what drives customer choices on Wish, offering insights that could be beneficial for e-commerce strategies.

## 2.   Prepare

### 2.1 About Dataset
The data we're using shows us a list of products that come up when you search for "summer" on Wish. The data was collected in early August 2020 and posted on the [Kaggle](https://www.kaggle.com) for education purposes. It includes information like how many people bought these products, their ratings, and prices, including discounts. This is just a small part of all the data Wish has, but it's still really useful.

[Click here to view the dataset on Kaggle](https://www.kaggle.com/datasets/jmmvutu/summer-products-and-sales-in-ecommerce-wish)

### 2.2 Data Organization 
The data consists of 2 datasets. All the datasets are in .csv file format and include long and wide formats. 
  *	computed_insight_success_of_active_sellers
  *	summer_products_with_rating_and_performance
For this project, we'll use *summer_products_with_rating_and_performance* dataset.

### 2.3 Data Limitations & Integrity 
#### 2.3.1 	Outdated Data
The summer clothes sales data was collected in August 2018 making the datasets outdated for current trend analysis. 
#### 2.3.2 	The Sample Size
The dataset might not represent the entire range of summer products available in the market. 
#### 2.3.3 	Accuracy and Reliability
There is no explicit indication of the source or method of data collection for these datasets, which raises questions about their accuracy and reliability. Any errors in data collection or recording would directly impact the analysis.

## 3.	Process

### 3.1 Import CSV Files into a SQL Database
### 3.2 Initial Data Overview
**Column Names and Row Count:**
Begin by identifying the column names and the total number of rows in the dataset. This initial step is essential for understanding the scope and scale of the data.

```
SELECT column_name
FROM `project.wish_sales_datasets.INFORMATION_SCHEMA.COLUMNS`
WHERE table_name = ‘summer_products_with_rating_and_performance’
ORDER BY ordinal_position;
```

Belows are **summer_products_with_rating_and_performance** dataset column names:
```
column names = [‘title_orig, price, retail_price, currency_buyer, units_sold, uses_ad_boosts, rating, rating_count, rating_five_count, rating_four_count, rating_three_count, rating_two_count, rating_one_count, tags, product_color
product_variation_size_id, product_variation_inventory, shipping_option_name, shipping_option_price, shipping_is_express, countries_shipped_to, inventory_total, has_urgency_banner, urgency_text, origin_country, merchant_title, merchant_name, merchant_info_subtitle, merchant_rating_count, merchant_rating, merchant_id, merchant_has_profile_picture, merchant_profile_picture, product_id, theme, crawl_month’]
```
**summer_products_with_rating_and_performance dataset** has **36 columns** and **1,573 rows in total.**

### 3.3 Plan Analysis Strategy
Based on the dataset structure and the information each column provides, our analysis strategy can be planed. This will include deciding which columns are relevant to our questions and what kind of insights we can derive from them.

Below are some of the analysis ideas that we can think of after looking at the obtained column names and number of rows:

 1.	Customer Preference Insights:
* Understand customer preferences based on product ratings (different rating counts) and the types of products sold (product_color, product_variation_size_id).
 2.	Price Analysis:
* Evaluate how price affects units sold and ratings.
* Study the effect of using ad boosts (uses_ad_boosts) on product sales and ratings.

### 3.4 Let’s clean the data
3.4.1 Remove duplicate data by using the DISTINCT (*) and create new tables that have no duplication. We will use these new tables in further analysis.
* summer_products_with_rating_and_performance dataset has 34 duplicate values and has been removed; 1539 unique values remain. 
* product_id column found 198 duplicate values and has been removed; 1,341 unique values remain in total.

3.4.2	Check for ‘NULL’ value in all datasets.
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

3.4.3	Replaced NULL values with zero
Since the free trial version of Bigquery doesn’t allow SET and UPDATE functions which can modify the data directly, we have to find another way by creating a new table instead.
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
3.4.4	Check for data inconsistency in product_color and product_variation_size_id
*	Run a query to list all distinct values in the product_color and product_variation_size_id columns. 
Since UPDATE function cannot be used in BigQuery. We correct data inconsistency in Excel.
*	Convert all text to lowercase.
*	Map similar colors to a standard color name (e.g., "light blue" "navy blue" “lake blue” “sky blue” to "blue"). Map the same category size description into the same group (e.g., “SIZE S” to “s”)


## 4.	Analyze and Share
In this project, we chose to combine the steps of analysis and sharing. This is because we think it will be simpler for people to understand the insights and findings if they see the analysis process that led to them. By doing this, we can speed up decision-making in practice, as we share discoveries in real-time rather than waiting until all the analysis is complete.
### 4.1 Customer Preference Data Analysis
This data analysis focuses on the impact of product_color on units sold (units_sold) and customer ratings (rating) to gain customer preference Insights.
<br><br> 4.1.1 Extract all columns that will be used in product variation analysis by using SQL.
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
4.2.2 Create a graph by using Tableau to see if there are any correlations between product color, unit sold and average rating.

<img width="800" alt="Screenshot 2567-03-12 at 14 48 35" src="https://github.com/tanya-tki/wish_sales_analysis/assets/153815515/e657a671-2b6e-4bf4-8e6e-72c1aaf3a11c">


<br>From the graph:
* Black products have the highest number of units sold, followed by white, blue, and green products.
* The average rating (blue line) fluctuates around 4 out of 5 across different product colors, suggesting that customers are generally satisfied with their purchases irrespective of color.
* The average rating does not appear to have a direct correlation with the number of units sold, as some less-sold colors still maintain a high rating.

<img width="484" alt="Screenshot 2567-03-12 at 14 50 40" src="https://github.com/tanya-tki/wish_sales_analysis/assets/153815515/275c1ce2-14e0-4dda-a2c3-f2867ca7d112">

In the second graph suggests a visual representation of the most common product colors available on the platform, ranked by how many products are available in each color:
* Again, black products lead in product count, followed by white, blue, red and red products, reflecting that merchants also stock preferebly these colors.

**Key insights:**
* Black is the most popular product color in terms of sales, substantially outselling other colors. 
* There is a consistent level of customer satisfaction across product colors, as indicated by the average rating. 
* There is no direct correlation between units sold and customer rating.
  
**Strategic Implications:**
* Interestingly, white and blue are the second most popular colors. If we can look at another dataset in different keywords (e.g., winter, spring, etc.), we might get a different color preference result. We can apply this data to how we advertise on the main page of the website to attract customer attention.

### 4.2	Price Data Analysis
#### 4.2.1 Average Units Sold and Ratings by Price Range Analysis
The objective of "Average Units Sold and Ratings by Price Range Analysis" is to understand how pricing affects sales volume and customer satisfaction, which can help inform better pricing and marketing strategies.
* Extract all columns that will be used in product variation analysis by using SQL. I grouped prices into 7 price ranges (‘0-5’, ’5-10’, ’10-15’, ’15-20’, ’20-25’, and ’25-30’).
```
SELECT
    CASE
        WHEN price BETWEEN 0 AND 4.99 THEN '0-5'
        WHEN price BETWEEN 5 AND 9.99 THEN '5-10'
        WHEN price BETWEEN 10 AND 14.99 THEN '10-15'
        WHEN price BETWEEN 15 AND 19.99 THEN '15-20'
        WHEN price BETWEEN 20 AND 24.99 THEN '20-25'
        WHEN price BETWEEN 25 AND 29.99 THEN '25-30'
        ELSE '30+'
    END AS price_range,
    ROUND(AVG(units_sold), 2) AS avg_units_sold,
    ROUND(AVG(rating), 2) AS avg_rating
FROM project.wish_sales_datasets.summer_products_with_rating_and_performance
GROUP BY price_range;
```
* Create a graph by using Tableau

<img width="536" alt="Screenshot 2567-03-12 at 15 02 20" src="https://github.com/tanya-tki/wish_sales_analysis/assets/153815515/e5b43717-8acd-4ea1-94f2-c56de346daea">


What we can interpret from the graph:

**1. Average Units Sold:**
* Products within the 5-10 EUR price bracket show the highest average units sold, indicating strong consumer preference for items in this cost-effective range.
* A marked decrease in average units sold is observed as we move to the 10-15 EUR range, continuing the downward trend for more expensive products.
* Notably, products in the 25-30 EUR range and beyond exhibit significantly lower sales volumes, suggesting a diminished market for higher-priced items.

**2. Average Rating:**
* The average rating generally increases as the price range increases, with the highest average rating in the over 30 EUR price range.
* This upward trend in ratings might imply greater customer satisfaction among higher-priced products. 
  
**Key Insights:**
* The trend suggests that affordability drives sales, with products priced between 5-10 EUR being the most purchased. This could be attributed to a perceived sweet spot of price-to-value ratio that appeals to a broader consumer base.
* A surprising trend shows that more expensive items get better ratings despite selling less. However, based on my observation, this assumption warrants skepticism. High ratings can often be associated with a low number of reviews, which may not accurately reflect broader consumer sentiment.

**Strategic Implications:**
* This analysis could guide e-commerce platforms and merchants in establishing pricing strategies. For instance, it could be beneficial to highlight products in the 5-10 EUR range, where consumer purchase frequency is highest.
* The platform might consider implementing measures to enhance the reliability of product ratings, such as verifying purchases or encouraging more comprehensive reviews, thereby aiding customers in making more informed decisions.

#### 4.2.2	Discount Sensitivity and Sales Impact Analysis
The objective of this analysis is to understand consumer sensitivity to price drops by analyzing how discounted prices affect units sold and product ratings.
* Calculate the discount percentage for each product using the formula:
(retail_price−price)/retail_price
<br> Note: retail_price is a price before discount.
	price is a price after discount.
* Divide the data into segments based on the discount percentage. We divide into 10 percentage increment brackets like 0-10%, 11-20%, and so on.
* For each discount segment, calculate the average units sold and the average rating.

```
  SELECT
  CASE
        WHEN ROUND(discount_percentage,2)  < 0.01 THEN 'no discount'
        WHEN ROUND(discount_percentage,2)  BETWEEN 0.01 AND 0.09 THEN '0-10%'
        WHEN ROUND(discount_percentage,2)  BETWEEN 0.1 AND 0.19 THEN '10-20%'
        WHEN ROUND(discount_percentage,2)  BETWEEN 0.2 AND 0.29 THEN '20-30%'
        WHEN ROUND(discount_percentage,2)  BETWEEN 0.3 AND 0.39 THEN '30-40%'
        WHEN ROUND(discount_percentage,2) BETWEEN 0.4 AND 0.49 THEN '40-50%'
        WHEN ROUND(discount_percentage,2) BETWEEN 0.5 AND 0.59 THEN '50-60%'
        WHEN ROUND(discount_percentage,2) BETWEEN 0.6 AND 0.69 THEN '60-70%'
        WHEN ROUND(discount_percentage,2) BETWEEN 0.7 AND 0.79 THEN '70-80%'
        ELSE 'more than 80%'
    END AS discount_percentage_range,
  AVG(units_sold) AS avg_units_sold,
  AVG(rating) AS avg_rating
FROM(
  SELECT
  product_id,
  retail_price,
  price,
  units_sold,
  rating,
  (retail_price - price) / retail_price AS discount_percentage, 
  (retail_price - price) AS discount_amount 
FROM polar-ring-331017.wish_sales_datasets.summer_products_with_rating_and_performance
) AS discounted_product
GROUP BY discount_percentage_range
ORDER BY discount_percentage_range;
```

The data below is the result of the sub-query. Interestingly, some products were set to sell higher at retail prices. Perhaps, some merchants are misunderstood? 

<img width="800" alt="Screenshot 2567-01-28 at 14 57 03" src="https://github.com/tanya-tki/wish_sales_analysis/assets/153815515/a22d0c78-33f2-4f37-b0e9-fb2a018c3afd">

<img width="538" alt="Screenshot 2567-03-12 at 15 03 18" src="https://github.com/tanya-tki/wish_sales_analysis/assets/153815515/211a21d3-45d6-48f7-b1fb-60226d849384">



What we can interpret from the graph:
1. Peak at No Discount and Deep Discounts: The graph shows high average units sold for products with no discount and for those with substantial discounts, particularly in the 30-40% range and more than 80% range.
2. Dip in Moderate Discounts: There's a noticeable dip in the average units sold for products with moderate discounts, especially in the 10-20% range.
3. Rating Stability: The average rating line remains relatively stable across all discount ranges, suggesting that the level of discount does not significantly affect customer satisfaction as reflected in product ratings.
   
**Insights:**
* Price Sensitivity Over Discount Sensitivity: The strong performance of non-discounted products suggests that consumers may be more sensitive to the absolute price rather than the discount itself. It's possible that products without discounts are already priced attractively enough to encourage purchases. This could be particularly true on a platform like Wish, where consumers are often looking for low-cost items.
* Effect of Large Discounts: The substantial increase in units sold at higher discount brackets may be due to the psychological effect where customers perceive they are getting a significant deal, which could trigger a sense of urgency to purchase.

**Strategic Implications:**
* Targeted Discounting: Since moderate discounts don't significantly boost sales, it may be more strategic to offer significant discounts selectively, ensuring they align with inventory goals, such as stock clearance.
* Profit Margin Balance: Given that deep discounts increase sales, it's important to balance these with profitability considerations to ensure overall business objectives are met.
* Avoiding Overuse of Discounts: Frequent large discounts could potentially condition customers to wait for sales, which could hurt the perceived value of the products and impact regular-priced sales negatively.

## 5.	Act

### Conclusion from Our Analysis:

The investigation into Wish's summer clothes sales data unveils crucial insights. 
* Black, white, blue, and green products had the highest sales, with black leading significantly.
* No direct correlation found between product ratings and sales volume.
* The 5-10 EUR price range saw the highest average units sold, indicating a consumer preference for moderately priced items.
* Higher-priced items (25-30 EUR and beyond) have lower sales volumes.

### Recommendations for Wish:

1. Product Assortment and Promotion: Given the strong preference for specific colors among consumers, We would advise Wish to enhance its promotional efforts by focusing on these preferred colors. This strategy can effectively leverage consumer tastes to boost the visibility and sales of these items, aligning marketing initiatives with established consumer preferences for better engagement and conversion rates.
2. Pricing Strategy: Adjusting prices to fit the 5-10 EUR range could match Wish users' expectations for affordable shopping. By advising sellers to set their prices within this range, it's likely to attract more buyers, possibly increasing sales. This approach uses the platform's strength in offering cost-effective options to meet customer demands for reasonably priced items, aiming to boost sales.
3. Customer Segmentation: Segment customers based on their sensitivity to price versus discounts. Tailor the shopping experience to these segments, showing the most price-sensitive customers the lowest-priced items and discount-seekers the best deals.
4. Enhanced Rating System: Improving the rating system could give customers better information since our analysis found no link between ratings and sales. A stronger system could make ratings more trustworthy, possibly helping customers make better buying choices.
To improve the rating system, consider these strategies:
* Verification of Purchases: Only allow ratings from verified purchasers to ensure authenticity.
* In-depth Review Options: Include options for detailed feedback, like product quality and delivery experience, to provide more comprehensive insights.
* Rating Moderation: Implement a system to flag and review suspicious or abusive ratings.
* Incentives for Reviews: Encourage honest reviews by offering incentives, ensuring a broader range of customer feedback.
* User Interface for Ratings: Make leaving reviews easy and intuitive, possibly impacting the quantity and quality of feedback received.
5. Marketing Adjustments: Tailor marketing campaigns to highlight the quality and affordability of products within the most popular price ranges and colors. Consider employing dynamic pricing strategies to adjust prices in real-time based on demand levels.
6. Enhanced Data Analysis: Continue leveraging data analytics to refine pricing, marketing, and stock strategies. Further research into customer reviews and feedback could provide deeper insights into purchasing decisions and product satisfaction.


### For future data analysis to enhance insights and strategic decisions at Wish:
1. Customer Demographics: Analyze age, location, and interests to tailor marketing and product selections.
2. Browsing Behaviors: Track time spent on the platform and pages visited to improve user experience and engagement.
3. Cart Abandonment Rates: Investigate the reasons behind cart abandonment to address issues related to pricing or user interface.
4. Product Return Rates and Feedback: Examine the reasons for returns and customer feedback for quality improvement and accurate product descriptions.
5. Seasonal Trends: Analyze sales data across different seasons to optimize inventory and marketing strategies.



