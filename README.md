# customer-segmentation

# Customer Segmentation using K-Means Clustering

## Project Overview

This project applies **customer segmentation using K-Means clustering** to identify different customer groups based on demographic characteristics, household information, income, spending behavior, and purchasing patterns.

The objective is not only to create customer clusters, but also to translate the results into **actionable business insights** that can support customer targeting, marketing campaigns, retention strategies, and resource allocation.

---

## Business Problem

Customers have different purchasing behaviors, spending levels, household characteristics, and preferred purchasing channels.

Applying the same marketing strategy to every customer may therefore result in inefficient marketing spending and lower campaign effectiveness.

This project aims to answer:

- What types of customers exist in the dataset?
- Which customer groups generate the highest value?
- How do purchasing behaviors differ across customer segments?
- What marketing strategies could be applied to each segment?

---

## Analysis Workflow

The project follows the workflow below:

1. Data Loading
2. Data Cleaning & Preprocessing
3. Feature Engineering
4. Exploratory Data Analysis (EDA)
5. Feature Scaling
6. Selecting the Number of Clusters
7. K-Means Clustering
8. Customer Profile Analysis
9. Business Interpretation
10. Marketing Recommendations

---

## Data Preparation

The dataset contains customer demographic and purchasing information, including:

- Income
- Age
- Household and family characteristics
- Product spending
- Web purchases
- Catalog purchases
- Store purchases
- Website visits
- Promotion responses

Data preprocessing and feature engineering were performed before applying the clustering model.

---

## Customer Segmentation

### Selecting the Number of Clusters

The **Elbow Method** was used to evaluate the appropriate number of customer segments.

Based on the distortion curve, **4 clusters** were selected for the K-Means clustering model.

```python
KMeans(n_clusters=4, random_state=0)

Customer Segment Profiles

The clustering analysis identified four customer groups with different economic and behavioral characteristics.

Cluster 0 — Family-Oriented Low-Spending Customers

Characteristics

Average income: ~42,738
Average spending: ~136
Average age: ~58
Larger household size
Higher number of children
Relatively low purchasing activity

Business Opportunity

Family bundles, value packages, discounts, and household-focused promotions may be more effective for this segment.

Cluster 1 — Active Mid-to-High Value Customers

Characteristics

Average income: ~61,795
Average spending: ~875
Strong web purchasing activity
Strong store purchasing activity
Uses multiple purchasing channels

Business Opportunity

This segment represents active customers with relatively high purchasing activity.

Cross-channel campaigns, loyalty programs, personalized recommendations, and upselling strategies could help increase customer lifetime value.

Cluster 2 — High-Value Customers

Characteristics

Average income: ~76,136
Average spending: ~1,371
Highest-value customer segment
Strong catalog and store purchasing activity
Very small number of children
Lower website visit frequency

Business Opportunity

Retention should be a priority for this segment.

VIP programs, premium products, exclusive offers, and personalized communication could help maintain engagement with these high-value customers.

Cluster 3 — Low-Value / Low-Engagement Customers

Characteristics

Average income: ~30,009
Average spending: ~108
Lowest-income customer segment
Relatively low purchasing activity
High website visit frequency
Low web purchasing activity

Business Opportunity

This segment presents an interesting online conversion opportunity.

Customers visit the website frequently but generate relatively few online purchases.

Targeted discounts, retargeting campaigns, personalized recommendations, and improvements to the online customer journey could potentially help convert browsing activity into purchases.

Key Business Insights
1. Customer Value Differs Significantly Across Segments

Clusters 1 and 2 represent the highest-value customer groups, while Clusters 0 and 3 have substantially lower spending levels.

This suggests that marketing investment should not necessarily be distributed equally across all customers.

2. High-Value Customers Require Retention Strategies

Cluster 2 contains customers with both the highest average income and highest average spending.

Retention programs and premium experiences may therefore be particularly important for this segment.

3. Online Engagement Does Not Always Lead to Purchases

Cluster 3 shows relatively high website visit frequency but low online purchasing activity.

This indicates a potential gap between customer engagement and conversion.

4. Household Characteristics Can Support Customer Targeting

Cluster 0 has a larger household profile and more children compared with other customer segments.

Family-oriented promotions may therefore be more relevant to this customer group.

Business Recommendations
Segment	Customer Strategy
Cluster 0	Family bundles, value promotions, household-focused offers
Cluster 1	Loyalty programs, cross-selling, upselling, cross-channel campaigns
Cluster 2	VIP programs, premium offers, personalized retention campaigns
Cluster 3	Retargeting, targeted discounts, online conversion campaigns

Customer segmentation allows businesses to move away from a one-size-fits-all marketing strategy and develop different approaches based on customer value and behavior.

Technologies Used
Python
Pandas
NumPy
Matplotlib
Seaborn
Scikit-learn
K-Means Clustering
Google Colab / Jupyter Notebook
Limitations & Next Steps

This project provides an initial customer segmentation framework.

Potential improvements include:

Evaluating cluster quality using metrics such as Silhouette Score
Comparing K-Means with other clustering algorithms
Incorporating additional transactional and behavioral data
Tracking customer movement between segments over time
Measuring campaign response for each customer segment
Developing a dashboard for customer segment monitoring

These improvements could help transform the segmentation model from exploratory analysis into a practical customer analytics tool for marketing decision-making.

Conclusion

This project demonstrates how unsupervised machine learning can be used not only to identify customer segments but also to support business decision-making.

By combining customer demographic information, spending behavior, purchasing channels, and household characteristics, four distinct customer groups were identified.

The results can support more targeted strategies for:

Customer retention
Marketing campaigns
Customer conversion
Loyalty programs
Marketing resource allocation

The project demonstrates the process of moving from:

Raw Customer Data → Data Analysis → Machine Learning → Customer Insights → Business Actions

日本語概要
顧客セグメンテーション分析

本プロジェクトでは、顧客の収入、購買金額、家族構成、購買チャネルなどのデータを用いて、K-Meansクラスタリングによる顧客セグメンテーションを行いました。

Elbow Methodを用いてクラスタ数を検討し、顧客を4つのグループに分類しました。

分析結果から、高価値顧客、ファミリー層、オンライン訪問頻度が高いものの購入につながっていない顧客など、各グループの特徴を明らかにしました。

さらに、分析結果をもとに、

VIP・ロイヤルティ施策
クロスセル・アップセル
ファミリー向けプロモーション
オンライン購入率向上施策

など、顧客グループごとのマーケティング施策を検討しました。

本プロジェクトを通して、データ分析および機械学習の結果を、ビジネス上の課題解決や意思決定につなげるプロセスを実践しました。
