DSCI 522 Avocado Predictors
================
Katie Birchard, Ryan Homer, Andrea Lee
02/08/2020

# Introduction

As millenials, we love avocados. However, as we all know, avocados can
be expensive. Therefore, we decided it would be interesting to
investigate what drives avocado prices. We would like to know what time
of year avocados are least expensive, and where avocados are the
cheapest. Hopefully, the results of this investigation can give us
insight into how we can enjoy our beloved avocado toasts without
breaking the bank.

We will be answering the research question: **What is the strongest
predictor of avocado prices in the United States?** Thus, our goal is to
find the feature that most strongly predicts the price of avocados in
the United States. A natural inferential sub-question would be to first
determine if any of the features correlate with avocado prices and if
there is any multicollinearity among the features. From our results, we
can then compute a rank of features by importance.

# Dataset

We analyzed the [avocado prices
dataset](https://www.kaggle.com/neuromusic/avocado-prices) retrieved
from Kaggle and compiled by the Hass Avocado Board using retail scan
data from the United States (Kiggins 2018). The dataset consists of
approximately 18,000 records over 4 years (2015 - 2018). The dataset
contains information about avocado prices, Price Look-Up (PLU) codes,
types (organic or conventional), region purchased in the United States,
volume sold, bags sold, and date sold.

# Analysis

We used a random forest regression model to determine the strongest
predictors of avocado prices. Before we fitted the model, we first
conducted a hypothesis test and a multicollinearity test to determine
which features are significant and should be used in the model. These
tests also identified features that are strongly correlated with one
another, and therefore would be redundant to include in the model.

The features we tested were:

  - `total_volume`: total volume of avocados sold
  - `PLU_4046`: number of of avocados with a price lookup code of 4046
    (small avocado) sold
  - `PLU_4225`: number of of avocados with a price lookup code of 4225
    (large avocado) sold
  - `PLU_4770`: number of of avocados with a price lookup code of 4770
    (x-large avocado) sold
  - `total_bags`: total number of bags of avocados sold
  - `small_bags`: number of small bags of avocados sold
  - `large_bags`: number of large bags of avocados sold
  - `xlarge_bags`: number of x-large bags of avocados sold
  - `type`: type of avocado sold (conventional or organic)
  - `year`: year avocado was sold in
  - `lat`: latitude of the U.S. region the avocado was sold in
  - `lon`: longitude of the U.S. region the avocado was sold in
  - `season`: season avocado was sold in

The features we used to train the random forest regression model on
were:

  - `type`: type of avocado sold (conventional or organic)
  - `lat`: latitude of the U.S. region the avocado was sold in
  - `lon`: longitude of the U.S. region the avocado was sold in
  - `season`: season avocado was sold in
  - *The intuition behind training on these selected features will be
    explained later on in the report.*

The target was:

  - `average_price`: average price of avocado sold

# Exploratory Data Analysis

We wanted to determine which features might be the most important to
include in our random forest regression model. Therefore we plotted
region, latitude, longitude, type, and season against the average price
to visualize the relationships between these variables (Figures 1-4). We
did not plot number of avocados sold from each of the PLU codes,
`PLU_4046`, `PLU_4225`, and `PLU_4770`, or the number of bags sold from
`total_bags`, `small_bags`, `large_bags`, and `xlarge_bags`, because the
relationship between avocado prices and avocados sold could be
reciprocal (i.e. avocados sold may influence the price and vice versa),
leading to a false interpretation. From looking at these relationships,
we can see that some regions, such as Hartford-Springfield and San
Francisco, have higher avocado prices than other regions, such as
Houston (Figure 1). When looking at latitude and longitude, it looks
like latitude has no observable trend with average price (Figure 2), but
longitude may have a slight parabolic trend with average price (Figure
3). We can also clearly see (and we may have already predicted from our
own experience) that organic avocados tend to be more expensive than
non-organic avocados(Figure 4A). Finally, when we observe the seasonal
trend of avocado prices, we can see that perhaps avocados are most
expensive in the fall months, and least expensive during the winter
months (Figure 4B).

![](../doc/img/EDA_region_plot.png) **Figure 1.** Distribution of the
average price of avocados in the United States by region.

![](../doc/img/EDA_lat_plot.png) **Figure 2.** Distribution of the
average price of avocados in the United States by latitude.

![](../doc/img/EDA_lon_plot.png) **Figure 3.** Distribution of the
average price of avocados in the United States by longitude.

![](../doc/img/EDA_type_season_plot.png) **Figure 4.** Distribution of
the average price of avocados in the United States by (A) type and (B)
season.

Since we want to ensure the prices in this dataset are relatively
accurate, we compared the average prices in this dataset to another
[study](https://www.statista.com/statistics/493487/average-sales-price-of-avocados-in-the-us/)
published by M. Shahbandeh in February 2019. According to the dataset we
selected, the average price of avocados from 2015 to 2018 was $1.41.
According to Shahbandeh’s study, the average price of avocados from 2015
to 2018 was $1.11 (Shahbandeh 2019). Thus, the average price from our
dataset is slightly higher compared to Shahbandeh’s study. This
discrepancy could be due to the inclusion of organic avocados in this
dataset, which tend to be more expensive. However, the prices are still
similar enough that the observations from this dataset are likely
accurate.

## Hypothesis Test

Before undergoing our main analysis, we first conducted a hypothesis
test to determine if any of the features are correlated with the target.
To conduct a hypothesis test, we fitted an additive linear model and
interpreted the p-values to determine which features are significant. We
chose a significance level of 0.05 as it is the industry standard. We
chose not to choose a stricter significance level (i.e. 0.01 or 0.001)
as we do not believe that predicting avocado prices requires as
conservative of a test.

Based on our EDA, we chose to fit a linear model to conduct our
hypothesis test. To confirm that a linear model would be appropriate for
this dataset, we examined its residual plot (Figure 5). Looking at the
residual plot below, the points are randomly distributed which indicates
that a linear model is appropriate in this case.

<div style="text-align: center">

<img src="../doc/img/residual_plot.png" width="680px" />

<div>

**Figure 5.** Residual plot to examine appropriateness of using a linear
model.

</div>

</div>

At a significance level of 0.05, it appears from the model below that
the following features are significant as their p-values are less than
the significance level (Table 1):

  - `type`
  - `year`
  - `lat`
  - `lon`
  - `season`
  - `total_volume`
  - `PLU_4046`
  - `PLU_4225`
  - `PLU_4770`

<table>

<caption>

**Table 1**. Hypothesis Test Table.

</caption>

<thead>

<tr>

<th style="text-align:left;">

term

</th>

<th style="text-align:right;">

estimate

</th>

<th style="text-align:right;">

std.error

</th>

<th style="text-align:right;">

statistic

</th>

<th style="text-align:left;">

p.value

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

(Intercept)

</td>

<td style="text-align:right;">

\-126.585

</td>

<td style="text-align:right;">

5.838

</td>

<td style="text-align:right;">

\-21.684

</td>

<td style="text-align:left;">

1.91e-102

</td>

</tr>

<tr>

<td style="text-align:left;">

total\_volume

</td>

<td style="text-align:right;">

0.000

</td>

<td style="text-align:right;">

0.000

</td>

<td style="text-align:right;">

\-2.683

</td>

<td style="text-align:left;">

0.00731

</td>

</tr>

<tr>

<td style="text-align:left;">

PLU\_4046

</td>

<td style="text-align:right;">

0.000

</td>

<td style="text-align:right;">

0.000

</td>

<td style="text-align:right;">

2.680

</td>

<td style="text-align:left;">

0.00738

</td>

</tr>

<tr>

<td style="text-align:left;">

PLU\_4225

</td>

<td style="text-align:right;">

0.000

</td>

<td style="text-align:right;">

0.000

</td>

<td style="text-align:right;">

2.687

</td>

<td style="text-align:left;">

0.00723

</td>

</tr>

<tr>

<td style="text-align:left;">

PLU\_4770

</td>

<td style="text-align:right;">

0.000

</td>

<td style="text-align:right;">

0.000

</td>

<td style="text-align:right;">

2.676

</td>

<td style="text-align:left;">

0.00746

</td>

</tr>

<tr>

<td style="text-align:left;">

total\_bags

</td>

<td style="text-align:right;">

\-0.030

</td>

<td style="text-align:right;">

0.040

</td>

<td style="text-align:right;">

\-0.754

</td>

<td style="text-align:left;">

0.451

</td>

</tr>

<tr>

<td style="text-align:left;">

small\_bags

</td>

<td style="text-align:right;">

0.031

</td>

<td style="text-align:right;">

0.040

</td>

<td style="text-align:right;">

0.758

</td>

<td style="text-align:left;">

0.449

</td>

</tr>

<tr>

<td style="text-align:left;">

large\_bags

</td>

<td style="text-align:right;">

0.031

</td>

<td style="text-align:right;">

0.040

</td>

<td style="text-align:right;">

0.758

</td>

<td style="text-align:left;">

0.449

</td>

</tr>

<tr>

<td style="text-align:left;">

xlarge\_bags

</td>

<td style="text-align:right;">

0.031

</td>

<td style="text-align:right;">

0.040

</td>

<td style="text-align:right;">

0.758

</td>

<td style="text-align:left;">

0.449

</td>

</tr>

<tr>

<td style="text-align:left;">

typeorganic

</td>

<td style="text-align:right;">

0.465

</td>

<td style="text-align:right;">

0.006

</td>

<td style="text-align:right;">

79.915

</td>

<td style="text-align:left;">

0

</td>

</tr>

<tr>

<td style="text-align:left;">

year

</td>

<td style="text-align:right;">

0.063

</td>

<td style="text-align:right;">

0.003

</td>

<td style="text-align:right;">

21.900

</td>

<td style="text-align:left;">

2.01e-104

</td>

</tr>

<tr>

<td style="text-align:left;">

lat

</td>

<td style="text-align:right;">

0.005

</td>

<td style="text-align:right;">

0.001

</td>

<td style="text-align:right;">

8.312

</td>

<td style="text-align:left;">

1.04e-16

</td>

</tr>

<tr>

<td style="text-align:left;">

lon

</td>

<td style="text-align:right;">

0.001

</td>

<td style="text-align:right;">

0.000

</td>

<td style="text-align:right;">

7.067

</td>

<td style="text-align:left;">

1.66e-12

</td>

</tr>

<tr>

<td style="text-align:left;">

seasonSpring

</td>

<td style="text-align:right;">

\-0.187

</td>

<td style="text-align:right;">

0.007

</td>

<td style="text-align:right;">

\-25.407

</td>

<td style="text-align:left;">

5.14e-139

</td>

</tr>

<tr>

<td style="text-align:left;">

seasonSummer

</td>

<td style="text-align:right;">

\-0.069

</td>

<td style="text-align:right;">

0.008

</td>

<td style="text-align:right;">

\-9.083

</td>

<td style="text-align:left;">

1.2e-19

</td>

</tr>

<tr>

<td style="text-align:left;">

seasonWinter

</td>

<td style="text-align:right;">

\-0.252

</td>

<td style="text-align:right;">

0.007

</td>

<td style="text-align:right;">

\-34.259

</td>

<td style="text-align:left;">

2.59e-246

</td>

</tr>

</tbody>

</table>

However, we should be cautious not to use the p-value significance as a
stand alone measure to determine if these features are correlated with
the target.

## Multicollinearity Test

Next, we conducted a multicollinearity test to check for any
redundancies between features. Under the assumption that the data can be
modelled linearly after observing the residual plot, we selected the
continuous numerical predictors, computed the correlation matrix, and
wrangled the data into a plottable dataframe (*Ggplot2 : Quick
Correlation Matrix Heatmap - R Software and Data Visualization*, n.d.)
(Figure 6).

<div style="text-align: center">

<img src="../doc/img/correlation_matrix.png" width="600px" />

<div>

**Figure 6.** Correlation matrix of continuous features.

</div>

</div>

Overall, there is fairly high collinearity between many of the
predictors. This was expected, since they all deal with volume or number
of avocados sold, be it by PLU code, bag type or total volume. In
particular, `total_bags` and `total_volume` were expected to be highly
correlated with other predictors that were sub-quantities of these
totals. Due to the high correlation, including all these predictors in a
prediction model would probably lead to overfitting.

To verify the result from the correlation matrix above, we also computed
the variance inflation (VIF) scores from the `car` package (Table 2).

<table class="table" style="margin-left: auto; margin-right: auto;">

<caption>

**Table 2.** Variance inflation scores of continuous features.

</caption>

<thead>

<tr>

<th style="text-align:left;">

name

</th>

<th style="text-align:right;">

value

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

total\_volume

</td>

<td style="text-align:right;">

3.78e+08

</td>

</tr>

<tr>

<td style="text-align:left;">

PLU\_4046

</td>

<td style="text-align:right;">

6.02e+07

</td>

</tr>

<tr>

<td style="text-align:left;">

PLU\_4225

</td>

<td style="text-align:right;">

5.32e+07

</td>

</tr>

<tr>

<td style="text-align:left;">

PLU\_4770

</td>

<td style="text-align:right;">

1.06e+06

</td>

</tr>

<tr>

<td style="text-align:left;">

total\_bags

</td>

<td style="text-align:right;">

1.25e+13

</td>

</tr>

<tr>

<td style="text-align:left;">

small\_bags

</td>

<td style="text-align:right;">

9.39e+12

</td>

</tr>

<tr>

<td style="text-align:left;">

large\_bags

</td>

<td style="text-align:right;">

6.58e+11

</td>

</tr>

<tr>

<td style="text-align:left;">

xlarge\_bags

</td>

<td style="text-align:right;">

1.32e+10

</td>

</tr>

</tbody>

</table>

The high VIF scores suggest extremely high collinearity for these
variables in a linear model. Therefore, we will be careful about using
these features as they are probably ineffective predictors of the
average avocado price.

# Results

## Random Forest

We fitted a random forest regressor model using the significant features
from the analysis above (`lat`, `lon`, `type`, and `season`). We used
one hot encoding to scale the categorical features (`type` and `season`)
and standard scaling tto scale the numerical features(`lat` and `lon`).
We used randomized cross validation to determine the optimal values for
maximum depth and number of estimators. We calculated the average
(validation) scores using cross validation to determine how well our
model was performing (Table 3).

<table>

<caption>

**Table 3**. Cross-validation scores for each of the folds in the random
forest regression model.

</caption>

<thead>

<tr>

<th style="text-align:right;">

Fold

</th>

<th style="text-align:right;">

Neg Mean Squared
    Error

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:right;">

1

</td>

<td style="text-align:right;">

\-0.06

</td>

</tr>

<tr>

<td style="text-align:right;">

2

</td>

<td style="text-align:right;">

\-0.07

</td>

</tr>

<tr>

<td style="text-align:right;">

3

</td>

<td style="text-align:right;">

\-0.06

</td>

</tr>

<tr>

<td style="text-align:right;">

4

</td>

<td style="text-align:right;">

\-0.12

</td>

</tr>

<tr>

<td style="text-align:right;">

5

</td>

<td style="text-align:right;">

\-0.14

</td>

</tr>

</tbody>

</table>

    ## [1] "The average cross-validation score for random forest regression is: 0.09"

From this model, we identified the relative importance of each feature
(Table 4).

<table>

<caption>

**Table 4**. The relative feature importances determined by random
forest regression model.

</caption>

<thead>

<tr>

<th style="text-align:left;">

Feature Names

</th>

<th style="text-align:right;">

Importance

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

Organic Type

</td>

<td style="text-align:right;">

0.42

</td>

</tr>

<tr>

<td style="text-align:left;">

Conventional Type

</td>

<td style="text-align:right;">

0.22

</td>

</tr>

<tr>

<td style="text-align:left;">

Longitude

</td>

<td style="text-align:right;">

0.19

</td>

</tr>

<tr>

<td style="text-align:left;">

Latitude

</td>

<td style="text-align:right;">

0.08

</td>

</tr>

<tr>

<td style="text-align:left;">

Fall Season

</td>

<td style="text-align:right;">

0.05

</td>

</tr>

<tr>

<td style="text-align:left;">

Summer Season

</td>

<td style="text-align:right;">

0.03

</td>

</tr>

<tr>

<td style="text-align:left;">

Winter Season

</td>

<td style="text-align:right;">

0.01

</td>

</tr>

<tr>

<td style="text-align:left;">

Spring Season

</td>

<td style="text-align:right;">

0.00

</td>

</tr>

</tbody>

</table>

According to the random forest regression, the top predictor of avocado
prices is `type` (i.e. whether the avocado is organic or conventional).
This result aligned with our expectations, as our EDA depicted
differences in distributions between organic and conventional avocado
prices.

## Linear Regression

To compare, we fitted a linear regression model using L2 regularization.
We also used randomized cross validation to determine the optimal value
for the complexity penalization factor, alpha. Again, we calculated the
average (validation) scores using cross validation to determine how well
our model was performing (Table 5).

<table>

<caption>

**Table 5**. Cross-validation scores for each of the folds in the linear
regression model.

</caption>

<thead>

<tr>

<th style="text-align:right;">

Fold

</th>

<th style="text-align:right;">

Neg Mean Squared
    Error

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:right;">

1

</td>

<td style="text-align:right;">

\-0.46

</td>

</tr>

<tr>

<td style="text-align:right;">

2

</td>

<td style="text-align:right;">

0.22

</td>

</tr>

<tr>

<td style="text-align:right;">

3

</td>

<td style="text-align:right;">

0.42

</td>

</tr>

<tr>

<td style="text-align:right;">

4

</td>

<td style="text-align:right;">

\-0.05

</td>

</tr>

<tr>

<td style="text-align:right;">

5

</td>

<td style="text-align:right;">

0.00

</td>

</tr>

</tbody>

</table>

    ## [1] "The average cross-validation score for linear regression is: 0.03"

The linear regression model had even lower error than the random forest
regression model. This may indicate that linear regression may be a
better model for predicting average avocado prices.

From the linear regression model, we also identified the relative
weights of each of the coefficients (Table 6).

<table>

<caption>

**Table 6**. The relative feature weights determined by the linear
regression model.

</caption>

<thead>

<tr>

<th style="text-align:left;">

Feature Names

</th>

<th style="text-align:right;">

Weights

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

Organic Type

</td>

<td style="text-align:right;">

0.25

</td>

</tr>

<tr>

<td style="text-align:left;">

Fall Season

</td>

<td style="text-align:right;">

0.12

</td>

</tr>

<tr>

<td style="text-align:left;">

Summer Season

</td>

<td style="text-align:right;">

0.05

</td>

</tr>

<tr>

<td style="text-align:left;">

Latitude

</td>

<td style="text-align:right;">

0.04

</td>

</tr>

<tr>

<td style="text-align:left;">

Longitude

</td>

<td style="text-align:right;">

0.04

</td>

</tr>

<tr>

<td style="text-align:left;">

Spring Season

</td>

<td style="text-align:right;">

\-0.06

</td>

</tr>

<tr>

<td style="text-align:left;">

Winter Season

</td>

<td style="text-align:right;">

\-0.11

</td>

</tr>

<tr>

<td style="text-align:left;">

Conventional Type

</td>

<td style="text-align:right;">

\-0.25

</td>

</tr>

</tbody>

</table>

When looking at the relative feature weights from the linear regression
model, we need to account for the absolute weight. Therefore, comparing
the two models, random forest regression and linear regression, we can
see that both indicated `type` as the most important predictor of
avocado pricing (Figure 7).

![](../results/feature_plot.png)

**Figure 7.** (A) Relative feature importances for each feature in
predicting average avocado prices determined by random forest
regression, and (B) relative feature weights for each feature in
predicting average avocado prices determined by linear regression.

# Discussion

## Conclusion

After creating and optimizing both a random forest regression model and
a regularized linear regression model to predict average avocado prices,
we found that the test scores for our predictive models were quite low.
For the random forest regression model, the test accuracy is **64%**.
The regularized linear regression model has an even lower accuracy of
**43%**.

Despite these disappointing test accuracy scores, we can still gain some
insight from these models. The random forest regression model predicted
that `type` is the most important feature for predicting avocado price.
This result is expected, since we observed a significant difference in
the distribution of average prices between organic and conventional
avocados during the exploratory data analysis and hypothesis testing. We
also expected this result from previous experience buying avocados.
Organic avocados are grown without the use of pesticides, and therefore
produce a lower yield per growing season, ultimately resulting in a more
expensive avocado.

The `region` feature also seemed to play some importance in the pricing
of avocados. For instance, regions such as Hartford-Springfield and San
Francisco were the third and fourth most important predictors of average
avocado price. It is unclear how these regions affect avocado prices. A
possible explanation could be that some regions, such as San Francisco,
have significantly higher priced avocados because of the fact that the
cost of living in these regions significantly higher than other regions
in the United States. On the other hand, other regions, such as Texas,
have significantly lower priced avocados because avocados are grown
these regions.

## Areas of Improvement

Our model could be improved by the inclusion of even more predictive
features or more data. Although it seems the predictors we did have
worked quite well, we could possibly gain even more insight from
predictors such as weather, grocery store chain, and popularity in the
media or public perception. In addition, the data that we do have only
spans four years. If we have data from a longer time span (e.g. 50
years), we might be able to more effectively determine the accuracy of
our chosen predictors.

We could also use more feature selection techniques to support the
conclusions that we made from our random forest and linear regression
models. Using techniques such as forward selection or recursive feature
selection may allow us to make stronger conclusions about which features
really do predict avocado prices.

# R and Python Packages

To perform this analysis, we used the R and Python programming languages
(R Core Team 2019; Van Rossum and Drake 2009). As well, the following R
packages were used: `broom` (Robinson and Hayes 2019), `caret` (Kuhn
2020), `car` (Fox and Weisberg 2019), `docopt` (de Jonge 2018),
`feather` (Wickham 2019), `ggpubr` (Kassambara 2018), `here`(Müller
2017), `kableExtra` (Zhu 2019), `knitr` (Xie 2014), `lubridate`
(Grolemund and Wickham 2011), `magick`(Ooms 2020), `RCurl` (Temple Lang
2020), `reshape2` (Wickham 2007), and `tidyverse` (Wickham et al. 2019).
The following Python packages were used: `altair` (Sievert 2018),
`numpy` (Oliphant 2006), `pandas` (McKinney and others 2010),
`pyarrow`(Team 2017), `scikit-learn` (Pedregosa et al. 2011), and
`selenium` (Salunke 2014). And the following OS package was used:
`chromedriver`(Google, n.d.).

# References

<div id="refs" class="references">

<div id="ref-docopt">

de Jonge, Edwin. 2018. *Docopt: Command-Line Interface Specification
Language*. <https://CRAN.R-project.org/package=docopt>.

</div>

<div id="ref-car">

Fox, John, and Sanford Weisberg. 2019. *An R Companion to Applied
Regression*. Third. Thousand Oaks CA: Sage.
<https://socialsciences.mcmaster.ca/jfox/Books/Companion/>.

</div>

<div id="ref-corr">

*Ggplot2 : Quick Correlation Matrix Heatmap - R Software and Data
Visualization*. n.d. STHTDA.
<http://www.sthda.com/english/wiki/ggplot2-quick-correlation-matrix-heatmap-r-software-and-data-visualization>.

</div>

<div id="ref-chromedriver">

Google. n.d. *ChromeDriver - Webdriver for Chrome*.
<https://chromedriver.chromium.org/downloads>.

</div>

<div id="ref-lubridate">

Grolemund, Garrett, and Hadley Wickham. 2011. “Dates and Times Made Easy
with lubridate.” *Journal of Statistical Software* 40 (3): 1–25.
<http://www.jstatsoft.org/v40/i03/>.

</div>

<div id="ref-ggpubr">

Kassambara, Alboukadel. 2018. *Ggpubr: ’Ggplot2’ Based Publication Ready
Plots*. <https://CRAN.R-project.org/package=ggpubr>.

</div>

<div id="ref-avocado-data">

Kiggins, J. 2018. “Avocado Prices: Historical Data on Avocado Prices and
Sales Volume in Multiple Us Markets.”
<https://www.kaggle.com/neuromusic/avocado-prices>.

</div>

<div id="ref-caret">

Kuhn, Max. 2020. *Caret: Classification and Regression Training*.
<https://CRAN.R-project.org/package=caret>.

</div>

<div id="ref-pandas">

McKinney, Wes, and others. 2010. “Data Structures for Statistical
Computing in Python.” In *Proceedings of the 9th Python in Science
Conference*, 445:51–56. Austin, TX.

</div>

<div id="ref-here">

Müller, Kirill. 2017. *Here: A Simpler Way to Find Your Files*.
<https://CRAN.R-project.org/package=here>.

</div>

<div id="ref-numpy">

Oliphant, Travis E. 2006. “A Guide to Numpy.” Trelgol Publishing USA.

</div>

<div id="ref-magick">

Ooms, Jeroen. 2020. *Magick: Advanced Graphics and Image-Processing in
R*. <https://CRAN.R-project.org/package=magick>.

</div>

<div id="ref-scikit-learn">

Pedregosa, F., G. Varoquaux, A. Gramfort, V. Michel, B. Thirion, O.
Grisel, M. Blondel, et al. 2011. “Scikit-Learn: Machine Learning in
Python.” *Journal of Machine Learning Research* 12: 2825–30.

</div>

<div id="ref-r">

R Core Team. 2019. *R: A Language and Environment for Statistical
Computing*. Vienna, Austria: R Foundation for Statistical Computing.
<https://www.R-project.org/>.

</div>

<div id="ref-broom">

Robinson, David, and Alex Hayes. 2019. *Broom: Convert Statistical
Analysis Objects into Tidy Tibbles*.
<https://CRAN.R-project.org/package=broom>.

</div>

<div id="ref-selenium">

Salunke, Sagar Shivaji. 2014. *Selenium Webdriver in Python: Learn with
Examples*. 1st ed. North Charleston, SC, USA: CreateSpace Independent
Publishing Platform.

</div>

<div id="ref-avocado-study">

Shahbandeh, M. 2019. “Average Sales Price of Avocados in the U.s.
2012-2018.”
<https://www.statista.com/statistics/493487/average-sales-price-of-avocados-in-the-us/>.

</div>

<div id="ref-2018-altair">

Sievert, Jacob VanderPlas AND Brian E. Granger AND Jeffrey Heer AND
Dominik Moritz AND Kanit Wongsuphasawat AND Arvind Satyanarayan AND
Eitan Lees AND Ilia Timofeev AND Ben Welsh AND Scott. 2018. “Altair:
Interactive Statistical Visualizations for Python.” *The Journal of Open
Source Software* 3 (32). <http://idl.cs.washington.edu/papers/altair>.

</div>

<div id="ref-pyarrow">

Team, Apache Arrow. 2017. *Pyarrow Documentation*.
<https://buildmedia.readthedocs.org/media/pdf/pyarrow/latest/pyarrow.pdf>.

</div>

<div id="ref-RCurl">

Temple Lang, Duncan. 2020. *RCurl: General Network (Http/Ftp/...) Client
Interface for R*. <https://CRAN.R-project.org/package=RCurl>.

</div>

<div id="ref-Python">

Van Rossum, Guido, and Fred L. Drake. 2009. *Python 3 Reference Manual*.
Scotts Valley, CA: CreateSpace.

</div>

<div id="ref-reshape2">

Wickham, Hadley. 2007. “Reshaping Data with the reshape Package.”
*Journal of Statistical Software* 21 (12): 1–20.
<http://www.jstatsoft.org/v21/i12/>.

</div>

<div id="ref-feather">

———. 2019. *Feather: R Bindings to the Feather ’Api’*.
<https://CRAN.R-project.org/package=feather>.

</div>

<div id="ref-tidyverse">

Wickham, Hadley, Mara Averick, Jennifer Bryan, Winston Chang, Lucy
D’Agostino McGowan, Romain François, Garrett Grolemund, et al. 2019.
“Welcome to the tidyverse.” *Journal of Open Source Software* 4 (43):
1686. <https://doi.org/10.21105/joss.01686>.

</div>

<div id="ref-knitr">

Xie, Yihui. 2014. “Knitr: A Comprehensive Tool for Reproducible Research
in R.” In *Implementing Reproducible Computational Research*, edited by
Victoria Stodden, Friedrich Leisch, and Roger D. Peng. Chapman;
Hall/CRC. <http://www.crcpress.com/product/isbn/9781466561595>.

</div>

<div id="ref-kableExtra">

Zhu, Hao. 2019. *KableExtra: Construct Complex Table with ’Kable’ and
Pipe Syntax*. <https://CRAN.R-project.org/package=kableExtra>.

</div>

</div>
