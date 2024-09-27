# Multiple Linear Regression Project
## Introduction
- In the competitive landscape of the retail industry, digital advertising plays a
pivotal role for brand visibility and consumer engagement. In the United States
advertisers spend approximately $72 billion in 2023 and it is projected to increase
every year. By 2028 it’s estimated that digital spend will be close to $90 billion [1].

- Social media is one of the pillar channels in digital marketing. Each platform has
massive reach and choosing the right target audience can make or break the
bottom line for a company. Social media advertising is a type of social media
marketing where companies use paid media to promote their business. Front
runners are YouTube, Facebook and instagram. Top ad spending vertical is
Retail and that is what this paper plans to explore [5].

- In this projectm we plan to explore data procured from a real world company about their ad
spend on two different platforms. The purpose of this project is to quantify the
impact of social media platforms on orders in an ecommerce marketing setting.

## Goals
- The primary objective of this project is to investigate the relationships between advertising metrics
including spend, clicks, impressions, frequency, reach (predictor variables) and their impact on orders
(target variable) for a popular ecommerce retail company.
- Build a predictive model that could forecast orders across multiple social media
advertising platforms.
- Answer the question, ‘If the company had one more dollar to spend on
marketing, would they invest it on facebook or instagram?’

## Data Source
The dataset is sourced from the Facebook (FB) API, offering a
comprehensive view of advertisement performance metrics.
The data was sourced from “Tuckernuck”, a leading
apparel company with permission. The data was collected via the Facebook
API provided in json format and parsed into a .csv file using node.js. It
encompasses advertisements placed on facebook and instagram on the same
dates but with different spend amounts. To ensure a balanced representation
across platforms, data from multiple years was included. However we were
unable to source more data as the API has a limitation of going back only 36
months.

- **API Documentation Link:** [Facebook API](https://developers.facebook.com/docs/marketing-api/insights/)
- Number of observations: 320

| Field Name | Data Type | Description |
|---|---|---|
| ad_name | varchar/not null | Name of the ad |
| ad_id | bigint/not null | Unique id of ad |
| platform | varchar/not null | Channel of ad display |
| spend | numeric | Total expenditure on each ad |
| clicks | integer | Number of clicks each ad receives |
| impressions | integer | Total number of times the ads were displayed |
| frequency | integer | Avg number of times each ad was shown to unique user |
| reach | integer | Total number of unique users who saw the ad |
| revenue | numeric | Revenue generated from each ad |
| orders | integer | Total number of orders |

## Data Preparation
Irrelevant features like ad_id and ad_name were dropped from the dataset.
There were no null values or instances containing zeros prior to the creation of a
dummy variable for the Social Media platform. Outliers were calculated with
interquartile range and 39 instances were removed.

## Multicollinearity Diagnostic
- **Variance Inflation Factor (VIF) Analysis** was performed and multicollinearity was
detected as several variables scored higher than 5 in the diagnostic. 
- Impressions and Reach variables were dropped due to extremely high scores, 94 and 95
respectively [2]

| VIF - Value | Conclusion |
|---|---|
| VIF = 1 | Not Correlated |
| 1 < VIF <= 5 | Moderately Correlated |
| VIF > 5 | Highly Correlated |

## Data Transformation
- Orders and Revenue were scaled logarithmically to adjust for skewness.
- To avoid multicollinearity, several variables were combined into ratios:
‘Clicks per Impression’ (cpi_rate) is the total ad spend divided by ad volume
(scaled by one thousand). [10]
- ‘Cost per Click’ (cpc_ratio) is a key metric of how efficiently the ad cost is
generating user interest. [11]
- ‘Cost per Impression’ (cpm_ratio) is the total ad spend divided by impressions
(scaled by one thousand).

## Modeling Techniques
**Multiple Linear Regression**
- When dealing with data that exhibits high multicollinearity, both Ridge and Lasso
regression techniques are more suitable alternatives to Multiple Linear
Regression (MLR), however MLR was still attempted to understand the
coefficients and p-values.

**Ridge Regression**
- Ridge regression (also known as Tikhonov regularization) addresses
multicollinearity by adding a penalty term to the sum of squared coefficients (L2
penalty).
- The effect is to shrink the coefficients, thereby reducing model
complexity and preventing overfitting. Ridge regression is particularly effective
when dealing with multicollinearity because it can handle situations where the
number of predictors exceeds the number of observations.
- However, it does not set coefficients to zero, meaning it does not perform variable selection and will
include all predictors in the final model, [8][10].

**Lasso Regression**
- Lasso regression (Least Absolute Shrinkage and Selection Operator) also modifies
the regression problem by adding a penalty equivalent to the absolute value of
the magnitude of the coefficients (L1 penalty).
- This has the effect of shrinking some coefficients to zero, effectively performing variable selection and
producing models that are simpler and more interpretable. Lasso can be
particularly useful when many predictors are irrelevant or if a sparse model is
desired.

## Evaluation Metrics
To compare the performance of Multiple Linear Regression (MLR), Ridge Regression,
and Lasso Regression and determine which model fits better, we used the below
common metrics.

| Model | R^2 | Adjusted R^2 | MSE |
|---|---|---|---|
| MLR | 0.243| 0.226 | 1.908 |
| Ridge | 0.6943 | 0.6791 | 8077.33 |
| Lasso | 0.6962 | 0.6810 | 8027.17 |

## Results
- We applied descriptive statistics to identify correlations and outliers for our data
set before building our predictive models. We noted a high degree of
correlation (i.e., multicollinearity) between multiple features [3,4,7]. 
- For example, impressions and reach were highly correlated with orders and revenue. The results of our Variance Inflation Factor (VIF) calculations confirmed this
observation as impressions and results had VIF scores of 94.14 and 95.422, respectively.
- Impressions and results were dropped from the model building
given these high VIF values. Next, outliers were removed from the target variable
(orders) based on IQR.
- Mixed linear regression (MLR) models are known to fail with data that is
correlated but we decided to try an MLR model for the experience and to
confirm its inappropriateness (Figure 6). As expected, the MLR performed poorly
with this data set as evidenced by an adjusted R
2 of 0.226.
- We next implemented Ridge and Lasso regression models for our data set, and both
methods apply a regularization, or penalty to constrain coefficients.
- The Lasso method has the added benefit of feature selection for model
simplification. The training and testing scores for the ridge model were 0.787 and
0.696, respectively. The lasso model obtained similar testing scores of 0.696 and
0.681 compared to ridge.
- Both models had adjusted R2 scores of ~0.68 that is significantly higher compared to the MLR model.
These results suggest the ridge and lasso models performed significantly better
compared to MLR. However, the ridge and lasso models did have much larger
MSE scores of ~8,000 compared to MLR values of 1.908 (Table 3). This is probably
due to the differences in scales used between the approaches.
- The coefficients for frequency and spend were similar between ridge (freq. = 64.9 & spend =
129.11) and lasso (freq. = 62.9 & spend = 134.7) models and appeared to have
the most influence on the models generated. In contrast, clicks had a much
lower coefficient (10.77 or 0) and predicted impact on the models.
- Unfortunately, ridge and lasso models appeared to be overfitting the data. The
potential problems for this overfitting are discussed below in conclusions [13].

## References
- [1]. Oberlo, ‘Social Media Ad Spend In The US (2017–2028)’, Accessed April,
15, 2024. [Link](https://www.oberlo.com/statistics/social-media-ad-spend)
- [2]. Liu, Z., Ma, X. (2019) Predictive Analysis of User Purchase Behavior based
on Machine Learning. International Journal of Smart Business and
Technology , 7 (1), 45-56
- [3]. Daoud, J. (2017) Multicollinearity and Regression Analysis [Review of
Multicollinearity and Regression Analysis]. Journal of Physics: Conference
Series, 949(012009). [Link](https://doi.org/10.1088/1742-6596/949/1/012009)
- [4]. Data, K. (2022). How to Solve Multicollinearity in Multiple Linear Regression
with OLS Method. KANDA DATA.
[Link](https://kandadata.com/how-to-solve-multicollinearity-in-multiple-linear-regression-with-ols-method/)
- [5]. Fildes, R., Ma, S., & Kolassa, S. (2019). Retail forecasting: Research and
Practice. International Journal of Forecasting, 38(4).
[Link](https://doi.org/10.1016/j.ijforecast.2019.06.004)
- [6]. Hübner, A., Hense, J., & Dethlefs, C. (2022). The revival of retail stores via
omnichannel operations: A literature review and research framework.
European Journal of Operational Research, 302(3).
[Link](https://doi.org/10.1016/j.ejor.2021.12.021)
- [7]. Oghenekevwe Etaga, H., Chibotu Ndubisi, R., & Lilian Oluebube, N. (2021).
Effect of Multicollinearity on Variable Selection in Multiple Regression. Science
Journal of Applied Mathematics and Statistics, 9(6), 141.
[Link](https://doi.org/10.11648/j.sjams.20210906.12)
- [8]. R Dennis Cook. (1998). Regression graphics: ideas for studying regressions
through graphics. Wiley. Online ISBN:9780470316931
- [9]. Saura, J. R. (2020). Using Data Sciences in Digital Marketing: Framework,
methods, and Performance Metrics. Journal of Innovation & Knowledge, 6(2),
92–102. Sciencedirect.
[Link](https://doi.org/10.1016/j.jik.2020.08.001)
- [10]. Singh, V. (2022). Understanding Ridge Regression Using Python - Shiksha
Online. Shiksha.com; Shiksha Online.
[Link](https://www.shiksha.com/online-courses/articles/understanding-ridge-regression-using-python/#:~:text=is%20Ridge%20Regression%3F)
- [11]. Li, H., Lee, K.-C., Xu, D., Shmakov, K., & Shen, W. (2023). Bid Optimization
for Offsite Display Ad Campaigns on eCommerce.
[Link](https://arxiv.org/abs/2306.10476)
- [12]. Yang, Y., Zhao, K., Zeng, D. D., & Jansen, B. J. (2022). Time-varying effects
of search engine advertising on sales–An empirical investigation in
E-commerce. Decision Support Systems, 163, 113843.
[Link](https://doi.org/10.1016/j.dss.2022.113843)
- [13]. Xue, Y. (2019) An overview of Overfitting and its Solutions. J. Phys.:Conf.
Ser. 1168, 022022
doi:10.1088/1742-6596/1168/2/022022


