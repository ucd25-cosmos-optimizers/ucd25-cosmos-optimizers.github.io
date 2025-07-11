---
title: "Mini Projects"
---
Here are some of the mini projects we created

## Introduction

The purpose of this study is to evaluate the correlation between **air quality** and **average income** across all Californian counties. The **parameter of concern** is all such counties in California. Our **null hypothesis** states that there is **no correlation between air quality of an area and that area’s average income**.

---

## Data

We collected data on **median AQI (Air Quality Index)** as an indicator of air pollution exposure by county and **2023 average income** per county in California.

<iframe src="/plotly/figure.html" width="100%" height="400px" style="border:none;"></iframe>

---

## Methodology

A **simple linear regression analysis** was performed using:

- **Independent variable:** 2023 average income per county  
- **Dependent variable:** Median AQI

We used this regression model to assess:

1. **Correlation:** The direction (positive or negative slope) and strength (R² value) of the relationship  
2. **Statistical significance:** p-value 

![Scatterplot of Median AQI vs. Average Income per County](/aqi_income_regression.png)

>The above plot visualizes the relationship between **median AQI** and **average income** for each Californian county, with the regression line overlaid.
---

## Discussion

The regression results showed that **R² = 0.007**, indicating that **less than 1% of the variation in median AQI is explained by income**. Additionally, we obtained a **p-value of 0.030**, suggesting that the relationship is **statistically significant at the 5% level**, despite the very weak explanatory power.

Thus, we are able to **reject the null hypothesis** that there is no correlation between the two variables and accept the **alternative hypothesis** that there is a correlation between income and the air quality of a particular region.

The regression line has a **slightly positive slope**, meaning that there is a **very weak positive correlation** between income and AQI. In other words, counties with **higher average incomes tended to have slightly higher AQI values** in this dataset compared to counties with lower average incomes.

This result **contradicts general environmental justice findings**, which often observe that **lower-income areas have worse air quality**. However, the extremely low R² suggests the correlation is **not practically significant**, and other confounding factors such as **urban density, traffic, and industrial zoning** likely drive air quality outcomes more than income alone.