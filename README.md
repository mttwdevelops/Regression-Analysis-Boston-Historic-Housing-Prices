# Boston Historic Housing Prices: Project Overview
- Take historic Boston housing pricing data and determine significant factors that lend towards home values
- Build regression model that is able to estimate home prices (MEDV variable) based off of significant factors
- Justify use of specific model

## Code and Resources Used
RStudio Version: R 4.1.0

Packages used: alr4, corrplot, ggplot2, lmtest

Boston Historic Housing Data: https://www.kaggle.com/datasets/fedesoriano/the-boston-houseprice-data

Topic introduction and data taken from:

Harrison, David & Rubinfeld, Daniel. (1978). Hedonic housing prices and the demand for clean
air. Journal of Environmental Economics and Management. 5. 81-102. 10.1016/0095-
0696(78)90006-2.

## Variables Read In
| Variable | Description |
| --- | --- |
| CRIM | Crime rate per capita by town [continuous] |
| ZN | Proportion of residential land zoned for lots [continuous] |
| INDUS | Proportion of non-retail business acres per town [continuous] |
| CHAS | Charles River dummy variable [discrete, 1 if tract bounds the river, 0 if not] |
| NOX | Nitrix Oxides concentration [continuous, PPM] |
| RM | Average number of rooms per dwelling [discrete] |
| AGE | Proportion of owner-occupied units build < 1940 [continuous] |
| DIS | Weighted distances to five major Boston employment centers [continuous] |
| RAD | Index of accessibility to radial highways [continuous] |
| TAX | Full value property tax rate per $10,000 [discrete] |
| PTRATIO | Pupil-Teacher ratio by area [continuous] |
| B | Proprtion of Black Americans by area [continuous] |
| LSTAT | Percent of lower status of the population [continuous] |
| MEDV | Median value of owner-occupied homes in $1,000's [discrete] |

It should be noted that B, or the proportion of Black Americans by area in the data, is a controversial inclusion by today's standards; however, since the dataset I am working with was released in the 1970s, I am including it for analysis purposes. It should also be noted that in 1968, anti-redlining laws were passed before the collection of the data; however, and unfortunately, it is hard to believe that racial discrimination towards minority property owners stopped.

## Data Cleaning / Selection
Since the data was already cleaned up for analysis, I began to explore the different distributions of the data as shown in the next section. I also did not remove any outliers, as I deemed every entry necessary for a fair analysis.

## Exploratory Data Analysis
All histograms and correlation plots are available in the report, but I will include important ones here:

Box and whisker plot of all variables (in read in order):
![boxwhiskall](https://github.com/mttwdevelops/Regression-Analysis-Boston-Historic-Housing-Prices/blob/main/Photos/allboxandwhiskerplot.png)

It appears that B has quite a few outliers that vary. This variable will be of further interest.

Histogram of the log transformation of MEDV (used log transformation to normalize right-skewed MEDV):
![histlogMEDV](https://github.com/mttwdevelops/Regression-Analysis-Boston-Historic-Housing-Prices/blob/main/Photos/logmedvhistplot.png)

Variables with moderate correlation to MEDV include INDUS, RM, NOX, TAX, PTRATIO, LSTAT. TAX and RAD also have a high correlation at 0.91. 

## Model Building
I used stepwise forward BIC analysis to determine which variables would be the best predictors for the change in MEDV between areas. I chose BIC values instead of AIC due to the large sample size of our data. Performing this analysis with a inverse, log, and square root transformed MEDV ultimately lead me to picking the log transformation, as it strikes a good balance in BIC score while also being normalized better compared to the alternatives. Multicollinearity assumption test was completed with a vairance inflation factors test, which resulted in a value < 10 for all of the model's variables, which is acceptable. 

| Model Type | BIC Score | R^2 Value |
| --- | --- | --- |
| 1/(MEDV) | -4311.18 | 0.6951 |
| log(MEDV) | -1672.25 | 0.8077 |
| sqrt(MEDV) | -832.83 | 0.8075 |

We can interpret the R^2 value as how well the model explains the variation in the data.

## Results
With the data I modeled on various statistics collected during 1970 in Boston’s surrounding neighborhoods, I conclude that the percentage of lower status individuals in the population, per capita crime rate, pupil-teacher ratio, average number of rooms per house, distance to the five major employment centers, nitric oxides concentration, Charles River closeness, proportion of Black Americans per area, index of accessibility to radial highways, and the full-value property tax rate per $10,000 of a dwelling are significant variables to consider when appraising the value of home values in the surrounding Boston area. Naturally, due to the age of the data, this model is not as accurate when trended with 2022 data, but the scope of this analysis is not meant on being super accurate, rather to practice regression techniques and variable analysis in R.
