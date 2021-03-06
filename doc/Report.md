DSCI 522 Analysis of PM 2.5 in Beijing and Shanghai
================
Ting Pan, Weishun Deng
November 22, 2018

Report
======

Introduction
------------

Particulate matter(PM) - also known as Atmospheric aerosol particles - are microscopic solid or liquid matter suspended in the atmosphere of Earth. PM2.5 are fine particles with a diameter of 2.5 micrometres or less. They have impacts on climate and precipitation that adversely affect human health. In other words, it's used as a measure of pollution. PM2.5 readings are often included in air quality reports from environmental authorities and companies.

This project is to conduct research on the inferential question - **Is the average PM2.5 in Beijing same as that in Shanghai?** We want to explore whether the average PM2.5 in Beijing is different from that in Shanghai in general.

The datasets we choose are [PM2.5 Data of Five Chinese Cities from Kaggle.com](https://www.kaggle.com/uciml/pm25-data-for-five-chinese-cities), which record PM2.5 of five Chinese cities during 2010 to 2015 each day each hour. Because we only care about PM2.5 in Beijing and Shanghai, these two raw datasets are as below:

*Table 1. Beijing PM2.5 Raw Dataset*

|   No|  year|  month|  day|  hour|  PM\_Dongsi|  PM\_Dongsihuan|  PM\_Nongzhanguan|  PM\_US.Post|
|----:|-----:|------:|----:|-----:|-----------:|---------------:|-----------------:|------------:|
|    1|  2010|      1|    1|     0|          NA|              NA|                NA|           NA|
|    2|  2010|      1|    1|     1|          NA|              NA|                NA|           NA|
|    3|  2010|      1|    1|     2|          NA|              NA|                NA|           NA|
|    4|  2010|      1|    1|     3|          NA|              NA|                NA|           NA|
|    5|  2010|      1|    1|     4|          NA|              NA|                NA|           NA|
|    6|  2010|      1|    1|     5|          NA|              NA|                NA|           NA|

There are four monitoring sites in Beijing:Dongsi, Dongsihuan, Nongzhanguan, US.Post.

*Table 2. Shanghai PM2.5 Raw Dataset*

|   No|  year|  month|  day|  hour|  PM\_Jingan|  PM\_US.Post|  PM\_Xuhui|
|----:|-----:|------:|----:|-----:|-----------:|------------:|----------:|
|    1|  2010|      1|    1|     0|          NA|           NA|         NA|
|    2|  2010|      1|    1|     1|          NA|           NA|         NA|
|    3|  2010|      1|    1|     2|          NA|           NA|         NA|
|    4|  2010|      1|    1|     3|          NA|           NA|         NA|
|    5|  2010|      1|    1|     4|          NA|           NA|         NA|
|    6|  2010|      1|    1|     5|          NA|           NA|         NA|

There are three monitoring sites in Shanghai:Jingan, US.Post, Xuhui.

As you can see from the heads of two tables above, there are a lot of missing data in our datasets. There are several kinds of missing data in our datasets:

-   There are ***no*** PM2.5 data at some specific hour.

-   There are missing data at monitoring sites and there are usually only one or two sites recording PM2.5 data. This is very typical in our datasets.

-   On some day, there are missing data for whole day.

To analyze the data efficiently, we need to deal the missing data carefully. We plan to do data wrangling on `Table 1` and `Table 2` as follows:

-   For each table, add a column `PM_Average` to record the average PM2.5 each day by removing all the missing data on that day and calculating the average of all non-NA values.

-   For each table, add a column `city` to indicate this categorical variable.

-   Combine the two tables into one, which is `Table 3`.

*Table 3. Beijing and Shanghai PM2.5 Tidy Dataset*

|  year|  month|  day|  PM\_Average| city     |
|-----:|------:|----:|------------:|:---------|
|  2010|      1|    1|     129.0000| Beijing  |
|  2010|      1|    1|           NA| Shanghai |
|  2010|      1|    2|     144.3333| Beijing  |
|  2010|      1|    2|           NA| Shanghai |
|  2010|      1|    3|      78.3750| Beijing  |
|  2010|      1|    3|           NA| Shanghai |

Visualization
-------------

To understand the tidy dataset, we take two plots to visualize it.

### Histogram

*Figure 1. Histograms of Beijing PM2.5 and Shanghai PM2.5*

![](../results/histogram.png)

`Figure 1.` shows the distributions of PM2.5 in Shanghai and Beijing. Both are right-skewed. Looking at the distribution of Beijing, the peak occurs at 25, and the data spread is from about 0 to 400. In contrast, the peak in distribution of Shanghai occurs at 50, which is larger than that of Beijing. The data spread of Shanghai is from 0 to 250, which is much narrower.

### Boxplot

*Figure 2. Boxplots of Beijing PM and Shanghai PM*

![](../results/boxplot.png)

`Figure 2.` helps analyze the relationship between the categorical variable `city` and the continuous variable `PM_Average`. We observe that the median PM2.5 of Beijing is higher than that of Beijing. Also, the boxplot of Shanghai is comparatively short, which suggests that overall PM2.5 values of Shanghai are denser. In addition, they both have much outliers, which reveals that there are a lot of ralatively high PM2.5 values detected in both cities.

Data Summary
------------

For each city, we can easily get the sample size, the sample mean, and the standard deviation of the sample mean. Then, assuming statistical independence of the two groups, the standard error of the mean of each city can be estimated as the sample standard deviation divided by the square root of the sample size `SE = s / sqrt(n)`. Additionally, we calculate 95% confidence interval of PM2.5 for each city using asymoptotic theory:

Confidence Interval = (mean - 1.96 \* SE, mean + 1.96 \* SE).

*Table 4. Summarize Beijing and Shanghai PM2.5 Tidy Dataset*

| city     |  number of sample|      mean|  standard deviation|  lower bound of C.I.|  upper bound of C.I.|
|:---------|-----------------:|---------:|-------------------:|--------------------:|--------------------:|
| Beijing  |              2155|  95.21643|           1.6473701|             91.98765|             98.44522|
| Shanghai |              1460|  54.81167|           0.9888218|             52.87362|             56.74973|

First thing we can find out from `Table 4` is that they have different sizes of non-NA data. There should be 2191 days bwtween Jan 1st 2010 to Dec 31st, 2015, however, PM2.5 only recorded 2155 days in Beijing and 1460 days in Shanghai.

We also find that the PM2.5 means of Beijing and Shanghai are totally different. Furthermore, we are 95% confident that the average PM2.5 in Beijing is between 91.98765 and 98.44522. Comparatively, we are 95% confident that the average PM2.5 in Shanghai is between 52.87362 and 56.74973.

Analysis and results
--------------------

### Hypothesis Testing

The null hypothesis: The average PM2.5 in Beijing and Shanghai are the same.

The alternative hypothesis: The average PM2.5 in Beijing and Shanghai are different.

We performed the Welch's two sample t-test intead of student t-test in this case. The main reason is that we have a lot of missing data in both of our datasets and the sizes of two samples are different so the homogeneous assumption of student t-test may not be hold. Another reason is that when we perform Welch's t-test we will not pool the sample standard deviations and this could give us a more accurate result.

The following is our test result:

*Table 5. Results of two-sample t-test*

|  estimate|  statistic|       p-value|  degree of freedom|  lower bound of C.I.|  upper bound of C.I.| method                  | alternative |
|---------:|----------:|-------------:|------------------:|--------------------:|--------------------:|:------------------------|:------------|
|  40.40476|   21.02933|  2.597003e-92|           3344.741|             36.63762|             44.17191| Welch Two Sample t-test | two.sided   |

As we can see in `Table 5`, p-value is extremely small and this is a strong evidence to reject the null hypothesis.

In conclusion, we reject the null hypothesis that there's no difference between the average PM2.5 in Beijing and Shanghai. This result is accord with our common sense of air conditions in China. Beijing has much more serious air pollution problems than other cities, so its PM2.5 level should be higher.

Beyond the project
------------------

There are still some limitations in our analysis.

First, due to the missing data, there are several issues about our test. We could not have an accurate average PM2.5 value for each day because lacking PM2.5 values at some specific times. Also, there are a lot of days missing PM2.5 values completely so we could not have symmetric datasets, let alone the assumptions of t-test.

Secondly, the datasets we used are time series data which means there exists dependencies in the datasets. There could be a strong relationship between today's PM2.5 and tomorrow's PM2.5. However, we didn't take any approach of time series analysis.

Additionally, we ignored many valuable features in the datasets due to time constraint. For example, we could include humidity and temperature of each day and create a regression model with PM2.5 value, to understand how the different features impact the response variable.

Reference
---------

-   UCI. "PM2.5 Data of Five Chinese Cities." RSNA Pneumonia Detection Challenge | Kaggle, 22 Aug. 2017, <http://www.kaggle.com/uciml/pm25-data-for-five-chinese-cities>.

-   "Welch's t-Test." Wikipedia, Wikimedia Foundation, 25 Oct. 2018, <http://en.wikipedia.org/wiki/Welch's_t-test>.
