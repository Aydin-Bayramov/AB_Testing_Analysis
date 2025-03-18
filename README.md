# A/B Testing of Themes Using Python

## Overview
A/B testing is a data-driven technique used to compare and evaluate different design elements, layouts, or themes in web applications. Instead of relying on assumptions, businesses can test variations on real users and analyze their impact on key metrics.

This project demonstrates A/B testing of website themes (Light vs. Dark) using Python. By analyzing user interaction data, we determine which theme performs better in terms of conversion rate, click-through rate, bounce rate, and session duration.

## Dataset
The dataset contains user interaction data, including:
- **Theme**: Light or Dark
- **Click Through Rate (CTR)**: Proportion of users clicking links or buttons
- **Conversion Rate**: Percentage of users signing up
- **Bounce Rate**: Percentage of users leaving after one page
- **Scroll Depth**: How far users scroll
- **Age**: User's age
- **Location**: User's location
- **Session Duration**: Total session time (in seconds)
- **Purchases**: Whether the user made a purchase
- **Added to Cart**: Whether the user added items to the cart

## Installation
Ensure you have Python installed, then install the required libraries:

```bash
pip install pandas plotly statsmodels scipy
```

## Usage
1. Load the dataset:
   ```python
   import pandas as pd
   df = pd.read_csv("website_ab_test.csv")
   df.head()
   ```

2. Check for missing values:
   ```python
   df.isnull().sum()
   ```

3. Statistical summary of the dataset:
   ```python
   df.describe()
   ```

4. Visualizing Click Through Rate (CTR) vs. Conversion Rate:
   ```python
   import plotly.express as px
   fig = px.scatter(df, x='Click Through Rate', y='Conversion Rate', color='Theme', trendline='ols')
   fig.write_image("graphs/scatter_ctr_conversion.png")
   fig.show()
   ```

5. A/B Test for Purchases using a Z-Test:
   ```python
   from statsmodels.stats.proportion import proportions_ztest
   
   light_conversions = light_theme_df[light_theme_df['Purchases'] == 'Yes'].shape[0]
   dark_conversions = dark_theme_df[dark_theme_df['Purchases'] == 'Yes'].shape[0]
   
   conversion_counts = [light_theme_conversions, dark_theme_conversions]
   sample_sizes = [light_theme_total, dark_theme_total]
   
   zstat, pval = proportions_ztest(conversion_counts, sample_sizes, alternative='two-sided')
   print("Z-statistic:", zstat, " P-value:", pval)
   ```

6. A/B Test for Session Duration using a T-Test:
   ```python
   from scipy import stats
   tstat, pval = stats.ttest_ind(light_theme_df['Session_Duration'], dark_theme_df['Session_Duration'], alternative='two-sided')
   print("T-statistic:", tstat, " P-value:", pval)
   ```

## Results
- The A/B test for purchases showed **no statistically significant difference** between the Light and Dark themes.
- The session duration test also indicated **no significant difference**, suggesting that user behavior remains consistent across both themes.

## Conclusion
Based on the statistical analysis, neither theme significantly outperforms the other. Companies may choose a theme based on aesthetic preference or other business considerations.
