Utilizing Multiple Regression to Predict Bike Rental Count Based on Seasonal and Environmental Factors

1\. Introduction

Bike-sharing systems have become increasingly  popular over the last 15 years. This trend is driven by greater awareness of environmental and health issues. These systems allow users to rent a bike from one location and return it to another. Owners can use predictions of daily or weekly rentals to decide how many bikes to have at stations at any time and how much insurance to buy. Prediction also helps identify the best times of year for ad campaigns.

The dataset for this analysis comes from Capital Bikeshare rentals in Washington DC in 2011 and 2012\. It is split into two sets: one for hourly rentals, one for daily rentals. The rental data is available at http://capitalbikeshare.com/system-data. Weather data is from http://www.freemeteo.com and holiday data from http://dchr.dc.gov/page/holiday-schedule.

The datasets consist of the following variables:

* Instant \- record index number  
* Dteday \- date  
* Season \- (1 \= Spring, 2 \= Summer, 3 \= Fall, 4 \= Winter)  
* Yr \- year (0 \= 2011, 1 \= 2012\)  
* Mnth \- month (1 \- 12\)  
* Hr \- hour (0 \-23)  
  * Note: This variable is not included in the Daily dataset  
* Holiday \- whether the specific day is a holiday (0 \= no, 1 \= yes)  
* Weekday \- day of the week (0 \- 6\)  
* Working day \- if the day is neither a holiday nor a weekend (1 \= yes, 0 \= no)  
* Weathersit \- weather conditions  
  * 1 \- Clear, Few clouds, Partly cloudy, Partly cloudy  
  * 2 \- Mist \+ Cloudy, Mist \+ Broken clouds, Mist \+ Few clouds, Mist  
  * 3 \- Light Snow, Light Rain \+ Thunderstorm \+ Scattered clouds, Light Rain \+ Scattered clouds  
  * 4 \- Heavy Rain \+ Ice Pallets \+ Thunderstorm \+ Mist, Snow \+ Fog  
* Temp \- normalized temperature in Celsius  
* Atemp \- normalized feeling temperature in Celsius   
* Hum \- normalized humidity  
* Windspeed \- normalized windspeed  
* Casual \- number of casual users  
* Registered \- number of registered users  
* CNT \- total count of users on a given day

All continuous environmental variables (temperature, humidity, windspeed) were normalized to a 0–1 scale in the provided dataset.

Because bikes are ridden and stored outside, both seasonal and environmental factors are essential for predicting bike rental counts. It can be assumed that rental counts will increase in more pleasant weather and decrease during bad weather. The following paper will develop a model to predict bike rental counts based on both daily and hourly usage. Detailed visuals and statistical results will be presented later in the Results section.

2\. Research Question

This study asks: To what extent can daily and hourly bike rental counts be predicted using environmental and seasonal factors, based on the available dataset?

To answer this, two modeling strategies will be evaluated. The first model uses only seasonal and environmental variables to address the primary question. The second model adds variables such as hour, weekday, year, and holiday for improved predictive performance. Both approaches will be examined in this paper.

3\. Methods

3.1 Data Preparation

Both day.csv and hour.csv were imported into SAS, and the variables were then defined as either categorical or continuous. Date, Season, Year, Month, Hour, Holiday, Weekday, Workingday, Weathersit, Casual, Registered, and Count have been defined as categorical; Temp, Atemp, Humidity, and Windspeed have been defined as continuous.

To properly run a regression, dummy variables were created for Season (1-4), Month (1-12), and Weather Situation (1-4). Hour (0-23) and Weekday (0-6) were created for the hourly models. To further ensure the best model for each dataset, transformations were also created for temperature (temp), atemp, humidity (hum), and wind speed (windspeed). This added the log, square, square root, and inverse of each continuous variable. Finally, reference categories Spring (season 1), January (month 1), and clear weather (weathersit 1\) were included in the intercept.

3.2 Modeling Approach

To estimate the regression model, the PROC REG procedure in SAS was used. A Stepwise regression was run to systematically evaluate all predictors. Each variable was tested for inclusion in the model, with p \< 0.05 required to remain in the equation. After each step, the previous variables were retested and dropped if their significance rose above p \< 0.05. This process continued until all variables had been tested and either included or excluded from the equation. To avoid multicollinearity, the VIF and tolerance for each variable were reviewed. If the VIF \> 5 or the tolerance \< 0.1, multicollinearity was indicated, and, excepting for polynomials, the variable was removed from the model. Both humidity and humidity^2 were retained due to the hierarchy principle. Finally, atemp was removed in the final models due to it’s colinearity with temp, while ln(temp) was retained for it’s non-linear fit.

In reviewing the residuals for the Daily models, the values appear to fall evenly around the count, indicating a good fit. However, when we examine the residuals for the Hourly dataset, a pattern becomes visible. Several transformations were applied to this dataset, with the best model displayed in the results.

Possible reasons for the patterns shown in the residuals will be reviewed in the Discussions and Limitations sections.

4\. Results

4.1 Descriptive Statistics Tables & Graphs

Tables 1 \- 6 describe the summary statistics of the continuous variables used in the regression analyses.

| Table 1 \- Daily Summary Statistics \- Original Continuous Variables |  |  |  |  |  |  |  |  |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Variable | N | Mean | Std Dev | Min | 25 % | Med | 75 % | Max |
| cnt | 731 | 4504.35 | 1937.21 | 22.00 | 3141.00 | 4548.00 | 5976.00 | 8714.00 |
| temp | 731 | 0.50 | 0.18 | 0.06 | 0.34 | 0.50 | 0.66 | 0.86 |
| atemp | 731 | 0.47 | 0.16 | 0.08 | 0.34 | 0.49 | 0.61 | 0.84 |
| hum | 731 | 0.63 | 0.14 | 0.00 | 0.52 | 0.63 | 0.73 | 0.97 |
| windspeed | 731 | 0.19 | 0.08 | 0.02 | 0.13 | 0.18 | 0.23 | 0.51 |

| Table 2 \- Daily Summary Statistics \- Seasonal & Environmental Continuous Variables |  |  |  |  |  |  |  |  |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Variable | N | Mean | Std Dev | Min | 25th Pctl | Med | 75th Pctl | Max |
| cnt | 731 | 4504.35 | 1937.21 | 22.00 | 3141.00 | 4548.00 | 5976.00 | 8714.00 |
| ln(temp) | 731 | \-0.78431 | 0.42964 | \-2.82799 | \-1.08742 | \-0.69649 | \-0.42248 | \-0.14889 |
| humidity | 731 | 0.63 | 0.14 | 0.00 | 0.52 | 0.63 | 0.73 | 0.97 |
| humidity^2 | 731 | 0.41 | 0.18 | 0.00 | 0.27 | 0.39 | 0.53 | 0.95 |
| windspeed | 731 | 0.19 | 0.08 | 0.02 | 0.13 | 0.18 | 0.23 | 0.51 |

| Table 3 \- Daily Summary Statistics \- Seasonal & Environmental Continuous Variables |  |  |  |  |  |  |  |  |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Variable | N | Mean | Std Dev | Min | 25% | Median | 75% | Max |
| cnt | 731 | 4504.35 | 1937.21 | 22.00 | 3152 | 4548 | 5956 | 8714 |
| ln(temp) | 731 | \-0.7843 | 0.4296 | \-2.828 | \-1.087 | \-0.696 | \-0.422 | \-0.1489 |
| hum | 731 | 0.6279 | 0.1424 | 0.000 | 0.520 | 0.627 | 0.730 | 0.970 |
| windspeed | 731 | 0.1905 | 0.0775 | 0.0224 | 0.135 | 0.181 | 0.233 | 0.5075 |

| Table 4 \- Hourly Summary Statistics \- Original Continuous Variables |  |  |  |  |  |  |  |  |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Variable | N | Mean | Std Dev | Min | 25% | Med | 75% | Max |
| cnt | 17,379 | 189.46 | 181.39 | 1.00 | 40.00 | 142.00 | 281.00 | 977.00 |
| temp | 17,379 | 0.50 | 0.19 | 0.02 | 0.34 | 0.50 | 0.66 | 1.00 |
| atemp | 17,379 | 0.48 | 0.17 | 0.00 | 0.33 | 0.48 | 0.62 | 1.00 |
| hum | 17,379 | 0.63 | 0.19 | 0.00 | 0.48 | 0.63 | 0.78 | 1.00 |
| windspeed | 17,379 | 0.19 | 0.12 | 0.00 | 0.10 | 0.19 | 0.25 | 0.85 |

| Table 5 \- Hourly Summary Statistics \- Seasonal & Environmental Continuous Variables |  |  |  |  |  |  |  |  |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Variable | N | Mean | Std Dev | Min | 25% | Median | 75% | Max |
| cnt | 17379 | 189.46 | 181.39 | 1 | 40 | 142 | 281 | 977 |
| ln(temp) | 17379 | \-1.0788 | 0.4661 | \-3.912 | \-1.422 | \-0.693 | \-0.415 | ≈0 |
| hum | 17379 | 0.6272 | 0.1929 | 0 | 0.48 | 0.63 | 0.78 | 1.00 |
| windspeed | 17379 | 0.1901 | 0.1223 | 0 | 0.1045 | 0.194 | 0.2537 | 0.8507 |

| Table 6 \- Hourly Summary Statistics \- Full Continuous Variables |  |  |  |  |  |  |  |  |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Variable | N | Mean | Std Dev | Min | 25% | Median | 75% | Max |
| cnt | 17379 | 189.46 | 181.39 | 1 | 40 | 142 | 281 | 977 |
| ln(temp) | 17379 | \-1.0788 | 0.4661 | \-3.912 | \-1.422 | \-0.693 | \-0.415 | ≈0 |
| hum | 17379 | 0.6272 | 0.1929 | 0 | 0.48 | 0.63 | 0.78 | 1.00 |
| windspeed | 17379 | 0.1901 | 0.1223 | 0 | 0.1045 | 0.194 | 0.2537 | 0.8507 |

					

Graph 1
![Graph 1: Histogram of Daily Bike Rentals](./img/graph1-daily_bike_count.png "Graph 1: Histogram of Daily Bike Count") 	
				
Graph 2
![Graph 2: Histogram of Hourly Bike Rentals](./img/graph2-hourly_bike_count.png "Graph 2: Histogram of Hourly Bike Rentals")

Graph 3
![Graph 3: Boxplot of Daily Bike Rentals by Weather](./img/graph3-daily_weather_boxblot.png "Graph 3: Boxplot of Daily Bike Rentals by Weather")					

Graph 4
![Graph 4: Boxplot of Daily Bike Rentals by Season](./img/graph4-hourly_weather_boxplot.png "Graph 4: Boxplot of Daily Bike Rentals by Season")

Graph 5	
![Graph 5: Boxplot of Hourly Bike Rentals by Weather](./img/graph5-daily_season_boxplot.png "Graph 5: Boxplot of Hourly Bike Rentals by Weather")				

Graph 6
![Graph 6: Boxplot of Hourly Bike Rentals by Season](./img/graph6-hourly_season_boxplot.png "Graph 6: Boxplot of Hourly Bike Rentals by Season")

4.2 Relationships Between Bike Count and Environmental Variables

To visualize the relationship between bike count and environmental variables, several scatterplots were created for both the Daily and Hourly datasets. These scatterplots are shown below in Graphs 7 \- 14\.

Graph 7
![Graph 7: Scatterplot of Daily Bike Count vs Temperature](./img/graph7-daily_temp_scatter.png "Graph 7: Scatterplot of Daily Bike Count vs Temperature")					

Graph 8
![Graph 8: Scatterplot of Hourly Bike Count vs Temperature](./img/graph8-hourly_temp_scatter.png "Graph 8: Scatterplot of Hourly Bike Count vs Temperature")

Graph 9					
![Graph 9: Scatterplot of Daily Bike Count vs Apparent Temperature](./img/graph9-daily_atemp_scatter.png "Graph 9: Scatterplot of Daily Bike Count vs Apparent Temperature")

Graph 10
![Graph 10: Scatterplot of Hourly Bike Count vs Apparent Temperature](./img/graph10-hourly_atemp_scatter.png "Graph 10: Scatterplot of Hourly Bike Count vs Apparent Temperature")

Graph 11					
![Graph 11: Scatterplot of Daily Bike Count vs Humidity](./img/graph11-daily_humidity_scatter.png "Graph 11: Scatterplot of Daily Bike Count vs Humidity")

Graph 12
![Graph 12: Scatterplot of Hourly Bike Count vs Humidity](./img/graph12-hourly_humidity_scatter.png "Graph 12: Scatterplot of Hourly Bike Count vs Humidity")

Graph 13					
![Graph 13: Scatterplot of Daily Bike Count vs Windspeed](./img/graph13-daily_windspeed_scatter.png "Graph 13: Scatterplot of Daily Bike Count vs Windspeed")

Graph 14
![Graph 14: Scatterplot of Hourly Bike Count vs Windspeed](./img/graph14-hourly_windspeed_scatter.png "Graph 14: Scatterplot of Hourly Bike Count vs Windspeed")

4.3 Model Performance

Tables 7 \- 10 include ANOVAs for each of the models

| Table 7 \- Daily  Seasonal & Environmental ANOVA |  |  |  |  |  |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Source | DF | Sum of Squares | Mean Square | F Value | Pr \> F |
| Model | 8 | 1636457524 | 204557191 | 133.89 | \<.0001 |
| Error | 722 | 1103077868 | 1527809 | — | — |
| Corrected Total | 730 | 2739535392 | — | — | — |

| Table 8 \-Daily Full Analysis of Variance |  |  |  |  |  |
| ----- | :---- | :---- | :---- | :---- | :---- |
| Source | DF | Sum of Squares | Mean Square | F Value | Pr \> F |
| Model | 13 | 2289293537 | 176099503 | 280.43 | \<.0001 |
| Error | 717 | 450241855 | 627952 | \- | \- |
| Corrected Total | 730 | 2739535392 | \- | \- | \- |

| Table 9 \- Hourly Seasonal & Environmental ANOVA |  |  |  |  |  |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Source | DF | Sum of Squares | Mean Square | F Value | Pr \> F |
| Model | 13 | 156626842 | 12048219 | 503.97 | \<.0001 |
| Error | 17365 | 415134750 | 23906 |  |  |
| Corrected Total | 17378 | 571761591 |  |  |  |

| Table 10 \- Hourly Full Analysis of Variance |  |  |  |  |  |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Source | DF | Sum of Squares | Mean Square | F Value | Pr \> F |
| Model | 43 | 388195481 | 9027802 | 852.54 | \<.0001 |
| Error | 17335 | 183566111 | 10589 |  |  |
| Corrected Total | 17378 | 571761591 |  |  |  |

Tables 11 \- 14 contain the Fit Statistics for each of the four models

| Table 11 \- Daily  Seasonal & Environmental Fit Statistics |  |
| ----- | ----- |
| Statistic | Value |
| Root MSE | 1236.04558 |
| Dependent Mean | 4504.34884 |
| Coefficient of Variation (Coeff Var) | 27.44116 |
| R-Square | 0.5973 |
| Adj R-Square | 0.5929 |

| Table 12 \- Daily Full Fit Statistics |  |
| ----- | :---- |
| Statistic | Value |
| Root MSE | 792.43446 |
| Dependent Mean | 4504.34884 |
| Coeff Var | 17.59265 |
| R-Square | 0.8357 |
| Adj R-Sq | 0.8327 |

| Table 13 \- Hourly Seasonal & Environmental Fit Statistics |  |
| ----- | :---- |
| Statistics | Value |
| Root MSE | 154.61697 |
| Dependent Mean | 189.46309 |
| Coeff Var | 81.60796 |
| R-Square | 0.2739 |
| Adj R-Sq | 0.2734 |

| Table 14 \- Hourly Full Fit Statistics |  |
| ----- | :---- |
| Statistics | Value |
| Root MSE | 102.90449 |
| Dependent Mean | 189.46309 |
| Coeff Var | 54.31374 |
| R-Square | 0.6789 |
| Adj R-Sq | 0.6782 |

4.4 Final Model Coefficients & Equations

log(Temperature) had the most potent positive effect on rentals in both the seasonal and full daily model, while humidity and windspeed had negative effects. Additionally, snowy and misty conditions had negative effects on the count, while the Winter and Summer seasons saw an increase in bike rental counts. 

The strongest predictors in the full hourly model were 7 AM to 9 AM and 4 PM to 6 PM. 5 PM showed the highest coefficient, with 384 rentals per hour. Final Model Coefficients and Parameters are listed in the appendix. 

Model 1: Daily \- Environmental and Seasonal Only

Count \= 6216.47923 \+ 598.908410\*(Summer) \+ 1044.57305\*(Winter) \+ 910.02922\*(September) \- 1008.14948\*(Snow or Heavy Mist) \+ 2894.19018\*log(Temperature) \- 7094.18446\*(Humidity) \- 8787.7995\*(Humidity^2) \- 3699.94771\*(Windspeed)

Model 2: Daily \- Full Model

Count \= 5660.52015 \+ 1976.38789\*(Year is 2012\) \+ 403.52571\*(Working Day) \+ 1078.20381\*(Summer) \+ 1018.46198\*(Fall) \+ 1418.69283\*(Winter) \+ 154.71973\*(March) \- 184.80862\*(Monday) \+ 480.46834\*(Saturday) \- 457.07392\*(Mist/Cloudy Weather) \- 1999.19452\*(Snow or Heavy Mist Weather) \+ 2102.74728\*log(Temperature) \- 1439.79826\*(Humidity) \- 3102.41688\*(Windspeed)Model 3: Hourly \- Environmental and Seasonal Only

Count \= 504.11202 \+ 22.37650\*(Summer) \+ 49.00903\*(Winter) \- 27.07037\*(April) \+ 2.40016\*(May) \- 31.92318\*(June) \- 34.25157\*(July) \- 5.45520\*(August) \+ 31.49073\*(September) \+ 4.56008\*(December) \+ 15.49138\*(Mist/Cloudy Weather) \+ 160.69453\*log(Temperature) \- 330.70216\*(Humidity) \+ 18.40851\*(Windspeed)

Model 4: Hourly \- Full Model

cnt \= 83.67942 \+ 85.16511\*(Year is 2012\) \- 15.39859\*(Holiday) \+ 12.77451\*(Working Day) \+ 56.18541\*(Summer) \+ 42.44138\*(Fall) \+ 57.28104\*(Winter) \+ 1.04177\*(February) \+ 13.27819\*(June) \+ 8.20373\*(July) \+ 21.82924\*(August) \+ 42.42841\*(September) \+ 30.26072\*(October) \- 17.77248\*(1 AM) \- 27.19965\*(2 AM) \- 38.12456\*(3 AM) \- 41.75827\*(4 AM) \- 25.09411\*(5 AM) \+ 34.16341\*(6 AM) \+ 170.13703\*(7 AM) \+ 311.78475\*(8 AM) \+ 165.10312\*(9 AM) \+ 111.65064\*(10 AM) \+ 138.32338\*(11 AM) \+ 178.98237\*(12 PM) \+ 174.99297\*(1 PM) \+ 160.02297\*(2 PM) \+ 169.68322\*(3 PM) \+ 231.26803\*(4 PM) \+ 384.22889\*(5 PM) \+ 351.16156\*(6 PM) \+ 241.08163\*(7 PM) \+ 160.28946\*(8 PM) \+ 109.83471\*(9 PM) \+ 72.08962\*(10 PM) \+ 32.65604\*(11 PM) \+ 3.96310\*(Friday) \+ 15.65782\*(Saturday) \- 11.82600\*(Mist/Cloudy Weather) \- 68.50032\*(Snow or Heavy Mist Weather) \+ 69.65795\*log(Temperature) \- 86.05864\*(Humidity) \- 39.69430\*(Windspeed)

| Model | Data Level | Predictors Included | R² | Adj R² | Best Use Case |
| :---- | :---- | :---- | :---- | :---- | :---- |
| Model 1 | Daily | Season & Weather Only | 0.5973 | 0.5929 | Broad seasonal trends |
| Model 2 | Daily | Full | 0.836 | 0.833 | Most accurate for daily forecasts |
| Model 3 | Hourly | Season & Weather Only | 0.274 | 0.273 | Weak model, missing time data |
| Model 4 | Hourly | Full | 0.679 | 0.678 | Best hourly predictor  |

4.5 Model Diagnostics

Residuals appear to be randomly distributed for the daily models (Graphs 15 \- 18), indicating that the assumptions of linearity and constant variance are reasonable. However, residuals for hourly models (Graphs 19 \- 22\)  showed a curved pattern, indicating possible non-linearity. 

Residuals vs Predicted – Daily Seasonal & Daily Full Model
  
Graph 15
![Graph 15: Residuals vs Predicted for Daily Seasonal Model](./img/graph15-daily_seasonal_residual.png "Graph 15: Residuals vs Predicted for Daily Seasonal Model")				

Graph 16
![Graph 16: Residuals vs Predicted for Daily Full Model](./img/graph16-daily_residual.png "Graph 16: Residuals vs Predicted for Daily Full Model")

Residuals vs Predicted – Hourly Seasonal vs Hourly Full Model

Graph 17
![Graph 17: Residuals vs Predicted for Hourly Seasonal Model](./img/graph17-hourly_seasonal_residual.png "Graph 17: Residuals vs Predicted for Hourly Seasonal Model")					

Graph 18
![Graph 18: Residuals vs Predicted for Hourly Full Model](./img/graph18-hourly_residual.png "Graph 18: Residuals vs Predicted for Hourly Full Model")

Actual vs Predicted – Daily Seasonal vs Daily Full Model

Graph 19
![Graph 19: Actual vs Predicted for Daily Seasonal Model](./img/graph19-daily_seasonal_residual_histogram.png "Graph 19: Actual vs Predicted for Daily Seasonal Model")					

Graph 20
![Graph 20: Actual vs Predicted for Daily Full Model](./img/graph20-daily_residual_histogram.png "Graph 20: Actual vs Predicted for Daily Full Model")

Actual vs Predicted – Hourly Seasonal vs Hourly Full Model

Graph 21	
![Graph 21: Actual vs Predicted for Hourly Seasonal Model](./img/graph21-hourly_seasonal_residual_histogram.png "Graph 21: Actual vs Predicted for Hourly Seasonal Model")				

Graph 22
![Graph 22: Actual vs Predicted for Hourly Full Model](./img/graph22-hourly_residual_histogram.png "Graph 22: Actual vs Predicted for Hourly Full Model")

4.6 Variable Transformation Effects

To improve linearity in the final model, several transformations were applied to the seasonal variables. Comparisons of the two models (Grpahs 23 \- 28\) show that the transformed versions better capture nonlinearity and reduce multicollineairty, as noted in the Variance Inflation Factors. 

Graph 23				
![Graph 23: Original Daily Bike Count vs Temperature](./img/graph23-daily_temp.png "Graph 23: Original Daily Bike Count vs Temperature")

Graph 24
![Graph 24: Transformed Daily Bike Count vs log(Temperature)](./img/graph24-daily_trasnformed_logtemp.png "Graph 24: Transformed Daily Bike Count vs log(Temperature)")

Graph 25					
![Graph 25: Original Daily Bike Count vs Humidity](./img/graph25-daily_humidity.png "Graph 25: Original Daily Bike Count vs Humidity")

Graph 26
![Graph 26: Transformed Daily Bike Count vs Humidity Squared](./img/graph26-daily_transformed_humiditysq.png "Graph 26: Transformed Daily Bike Count vs Humidity Squared")

Graph 27
![Graph 27: Original Hourly Bike Count vs Temperature](./img/graph27-hourly_temp.png "Graph 27: Original Hourly Bike Count vs Temperature")					

Graph 28
![Graph 28: Transformed Hourly Bike Count vs log(Temperature)](./img/graph28-hourly_transformed_logtemp.png "Graph 28: Transformed Hourly Bike Count vs log(Temperature)")

5\. Discussion

The most significant predictor variables in the seasonal models were temperature, humidity, windspeed, and season. Temperature had a very strong positive relationship with bike rental count, with rentals increasing sharply before leveling out at higher temperatures. Humidity and wind speed have negative relationships with bike rental counts, indicating that higher humidity and wind speed are associated with fewer bike rentals. Both the Summer and Winter seasons had a positive relationship with bike rentals as well. While higher temperatures may explain the increase seen in the summer, increased rentals in the Winter may be due to other variables, such as the holiday season.

‘Hour’ is shown to be a robust predictor in the hourly models due to the high volume of traffic typically observed at specific hours of the day. When reviewing the full hourly model, the typical commuting times emerge as the most significant factors in predicting hourly bike rentals. A noticeable spike is observed from 7:00 AM to 9:00 AM, as well as from 4:00 PM to 6:00 PM. Additionally, because the weather can vary significantly from hour to hour, seasonal and environmental variables were weaker in the hourly models.

When comparing the environmental-only models to the full models, the environmental ones are shown to be significantly weaker. The Daily Seasonal accounted for 59.21% of the variance, while the Full Daily model only accounted for 83.57%. Similarly, the Seasonal Hour model accounted for only 27.39% of the variance, while the Full Hourly model accounted for 67.89%. When non-seasonal or environmental factors are removed from the equation, R² drops by about 25% for the Daily model and by over 40% for the Hourly model. This is because factors such as the day of the week or the hour of the day have a significant impact on bike rental counts. There may be an increase in rentals on the weekend for the daily model, or, as discussed above, commuting hours for the hourly model. 

Practical implications for these models include commuting, weather-related planning, bike redistribution, and demand pricing. On days with fairer weather, users are more likely to rent bikes. Bike rental companies may want to make more bikes available on those days, and may implement some form of surge pricing during common commuting hours. Conversely, rental companies may choose to move more of their bikes inside on days where rain and storms are forecasted and rental counts are predicted to be lower as a cost saving measure. 

6\. Limitations

One main limitation to this study is that linear regression assumptions may not hold perfectly. While the Daily full model results in a fairly good fit for the regression model, the Hourly full model is more limited in its ability to explain the variance in the data. In particular, the hourly model shows a significant skew in bike rental count, and when limited to seasonal and environmental factors only, is only able to explain about 27% of the variance. Due to the skewness of the hourly data, a different type of regression, such as a Poisson or Random Forest model might provide a better fit. However, that is beyond the scope of this paper. 

There was also no handling of time series autocorrelation. As a result, underlying patterns that may have resulted from seasonal or weekly changes may have been missed. Because autocorrelation can also aid in model selection, a better regression method may have been more easily identified for the Hourly Dataset. 

Events like Hurricane Sandy and the DC Protests were also not included in the dataset. Hurricane Sandy would likely have resulted in lower bike rental counts, while the DC Protests may have resulted in an increased count of bike rentals. Because both of these events occurred in the fall, seasonal bike rental counts may be skewed and not correctly reflect bike rental counts in an average fall season. 

Finally, the continuous variables in this dataset \- temp, atemp, hum, and windspeed \- have all been normalized, with maximum values set. If the raw data had been included in the day and hour dataset, the results may have shown a different distribution. Additionally, these variables may not have had to be transformed as part of the bike rental prediction if the raw numbers were made available. 

7\. Conclusion

The research question was whether seasonal and environmental variables can be used to help predict bike rental counts. From the results and final models, it is evident that while these variables can be helpful in predictions, they are not adequate predictors on their own. They must be included with factors like weekday, whether that day was a work day, and hour of the day to provide an accurate prediction of bike rental count.

The factors that most influence daily bike rentals include whether that day was a working day, and the season of the rental, while the factors that most influenced the hourly bike rentals were the hours themselves. Together, these factors indicate that working days and commuting hours are strong predictors of bike rental count. Of the four models, the Full Daily model performed the best at predicting bike rental count, explaining 83.57% of the variance in bike rentals. 

Due to the weakened predictive power of the seasonal and environmental regression models \- particularly the seasonal hourly model \- a different model, such as Random Forest or Poisson regression may provide a better predictive model in future analyses. 

8\. Appendix – SAS Code

Parameter Estimates

| Daily Seasonal & Environmental Parameter Estimates |  |  |  |  |  |  |  |
| ----- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Variable | DF | Parameter Estimate | Standard Error | t Value | Pr \> |t| | Tolerance | Variance Inflation |
| Intercept | 1 | 8445.0354 | 214.80684 | 39.31 | \<.0001 | . | 0 |
| Summer | 1 | 565.28262 | 117.43456 | 4.81 | \<.0001 | 0.81406 | 1.22841 |
| Winter | 1 | 1060.12694 | 116.63108 | 9.09 | \<.0001 | 0.84388 | 1.18501 |
| September | 1 | 867.22401 | 178.24549 | 4.87 | \<.0001 | 0.88337 | 1.13203 |
| Mist/Snow | 1 | \-1378.65558 | 299.53419 | \-4.6 | \<.0001 | 0.84466 | 1.18391 |
| ln(temp) | 1 | 2944.97571 | 115.40877 | 25.52 | \<.0001 | 0.86125 | 1.16111 |
| Humidity² | 1 | \-3154.99394 | 291.16146 | \-10.84 | \<.0001 | 0.77381 | 1.29231 |
| Wind Speed | 1 | \-3964.24144 | 633.49138 | \-6.26 | \<.0001 | 0.87852 | 1.13828 |

| Daily Full Model Parameter Estimates  |  |  |  |  |  |  |  |
| ----- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Variable | DF | Parameter Estimate | Standard Error | t Value | Pr \> |t| | Tolerance | Variance Inflation |
| Intercept | 1 | 5660.52015 | 293.55487 | 19.28 | \<.0001 | . | 0 |
| Year 2012 | 1 | 1976.38789 | 59.76446 | 33.07 | \<.0001 | 0.96202 | 1.03948 |
| Working Day | 1 | 403.52571 | 79.78915 | 5.06 | \<.0001 | 0.62427 | 1.60186 |
| Summer | 1 | 1078.20381 | 111.535 | 9.67 | \<.0001 | 0.36662 | 2.72762 |
| Fall | 1 | 1018.46198 | 138.23722 | 7.37 | \<.0001 | 0.23531 | 4.24975 |
| Winter | 1 | 1418.69283 | 99.87386 | 14.2 | \<.0001 | 0.46751 | 2.13897 |
| March | 1 | 154.71973 | 112.42568 | 1.38 | 0.1692 | 0.87558 | 1.1421 |
| Monday | 1 | \-184.80862 | 85.09595 | \-2.17 | 0.0302 | 0.96441 | 1.0369 |
| Saturday | 1 | 480.46834 | 105.547 | 4.55 | \<.0001 | 0.62689 | 1.59519 |
| Mist / Cloudy | 1 | \-457.07392 | 78.36426 | \-5.83 | \<.0001 | 0.62527 | 1.539932 |
| Mist/Snow | 1 | \-1999.19452 | 200.31868 | \-9.98 | \<.0001 | 0.76723 | 1.3034 |
| ln(temp) | 1 | 2102.74728 | 116.91104 | 17.99 | \<.0001 | 0.34095 | 2.93301 |
| Humidity | 1 | \-1439.79826 | 287.45773 | \-5.01 | \<.0001 | 0.51317 | 1.94868 |
| Wind Speed | 1 | \-3102.41688 | 413.18644 | \-7.51 | \<.0001 | 0.83894 | 1.19198 |

| Hourly Seasonal & Environmental Parameter Estimates |  |  |  |  |  |  |  |
| ----- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Variable | DF | Parameter Estimate | Standard Error | t Value | Pr \> |t| | Tolerance | Variance Inflation |
| Intercept | 1 | 504.11202 | 7.27465 | 69.3 | \<.0001 | . | 0 |
| Summer | 1 | 22.3765 | 5.54512 | 4.04 | \<.0001 | 0.23629 | 4.23217 |
| Winter | 1 | 49.00903 | 3.5209 | 13.92 | \<.0001 | 0.60236 | 1.66013 |
| April | 1 | \-27.07037 | 6.89851 | \-3.92 | \<.0001 | 0.38109 | 2.62404 |
| May | 1 | 2.40016 | 7.1653 | 0.33 | 0.7377 | 0.34223 | 2.92203 |
| June | 1 | \-31.92318 | 6.40794 | \-4.98 | \<.0001 | 0.44084 | 2.26841 |
| July | 1 | \-34.25157 | 6.09092 | \-5.62 | \<.0001 | 0.47361 | 2.11145 |
| August | 1 | \-5.4552 | 5.95016 | \-0.92 | 0.3593 | 0.50025 | 1.99902 |
| September | 1 | 31.49073 | 5.3623 | 5.87 | \<.0001 | 0.63072 | 1.58549 |
| December | 1 | 4.56008 | 4.52174 | 1.01 | 0.3132 | 0.86198 | 1.16012 |
| Mist / Cloudy | 1 | 15.49138 | 2.75587 | 5.62 | \<.0001 | 0.93796 | 1.06614 |
| ln(temp) | 1 | 160.69453 | 4.0223 | 39.95 | \<.0001 | 0.39147 | 2.5545 |
| Humidity | 1 | \-330.70216 | 6.69057 | \-49.43 | \<.0001 | 0.82564 | 1.21119 |
| Wind Speed | 1 | 18.40851 | 10.18599 | 1.81 | 0.0707 | 0.88587 | 1.12884 |

| Hourly Full Parameter Estimates |  |  |  |  |  |  |  |  |
| ----- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Variable | Label | DF | Parameter Estimate | Standard Error | t Value | Pr \> |t| | Tolerance | Variance Inflation |
| Intercept | Intercept | 1 | 83.67942 | 7.84842 | 10.66 | \<.0001 | . | 0 |
| Year 2012 | Year 2012 | 1 | 85.16511 | 1.58271 | 53.81 | \<.0001 | 0.973 | 1.02775 |
| holiday | holiday | 1 | \-15.39859 | 5.07361 | \-3.04 | 0.0024 | 0.84712 | 1.18048 |
| Working Day | Working Day | 1 | 12.77451 | 2.32279 | 5.5 | \<.0001 | 0.52136 | 1.91805 |
| Summer |  | 1 | 56.18541 | 3.18713 | 17.63 | \<.0001 | 0.31682 | 3.15637 |
| Fall |  | 1 | 42.44138 | 5.12418 | 8.28 | \<.0001 | 0.121 | 8.26417 |
| Winter |  | 1 | 57.28104 | 3.12368 | 18.34 | \<.0001 | 0.33899 | 2.94993 |
| February |  | 1 | \-0.07713 | 3.56556 | \-0.02 | 0.9827 | 0.67307 | 1.48573 |
| March |  | 1 | 6.08176 | 3.16819 | 1.92 | 0.0549 | 0.78254 | 1.27789 |
| June |  | 1 | 13.27819 | 3.63289 | 3.65 | 0.0003 | 0.60753 | 1.64601 |
| July |  | 1 | 8.20373 | 5.40826 | 1.52 | 0.1293 | 0.26609 | 3.75817 |
| August |  | 1 | 21.82924 | 5.35643 | 4.08 | \<.0001 | 0.27343 | 3.65726 |
| September |  | 1 | 42.42841 | 4.51 | 9.41 | \<.0001 | 0.39495 | 2.53198 |
| October |  | 1 | 30.26072 | 3.50163 | 8.64 | \<.0001 | 0.64942 | 1.53984 |
| 1 AM |  | 1 | \-17.77248 | 5.40577 | \-3.29 | 0.001 | 0.52227 | 1.91472 |
| 2 AM |  | 1 | \-27.19965 | 5.42489 | \-5.01 | \<.0001 | 0.52484 | 1.90534 |
| 3 AM |  | 1 | \-38.12456 | 5.46333 | \-6.98 | \<.0001 | 0.53027 | 1.88583 |
| 4 AM |  | 1 | \-41.75827 | 5.46773 | \-7.64 | \<.0001 | 0.52942 | 1.88887 |
| 5 AM |  | 1 | \-25.09411 | 5.43201 | \-4.62 | \<.0001 | 0.52207 | 1.91546 |
| 6 AM |  | 1 | 34.16341 | 5.41855 | 6.3 | \<.0001 | 0.51912 | 1.92632 |
| 7 AM |  | 1 | 170.17303 | 5.40971 | 31.46 | \<.0001 | 0.51945 | 1.92511 |
| 8 AM |  | 1 | 311.78475 | 5.40385 | 57.7 | \<.0001 | 0.52058 | 1.92094 |
| 9 AM |  | 1 | 165.10312 | 5.40789 | 30.53 | \<.0001 | 0.5198 | 1.92381 |
| 10 AM |  | 1 | 111.65064 | 5.42598 | 20.58 | \<.0001 | 0.51634 | 1.93671 |
| 11 AM |  | 1 | 138.32338 | 5.45945 | 25.34 | \<.0001 | 0.51003 | 1.96067 |
| 12 PM |  | 1 | 178.98237 | 5.49753 | 32.56 | \<.0001 | 0.50233 | 1.99074 |
| 1 PM |  | 1 | 174.99297 | 5.52697 | 31.66 | \<.0001 | 0.49634 | 2.01475 |
| 2 PM |  | 1 | 160.02297 | 5.55056 | 28.83 | \<.0001 | 0.49213 | 2.03199 |
| 3 PM |  | 1 | 169.68322 | 5.55853 | 30.53 | \<.0001 | 0.49072 | 2.03783 |
| 4 PM |  | 1 | 231.26038 | 5.54806 | 41.68 | \<.0001 | 0.49193 | 2.03283 |
| 5 PM |  | 1 | 384.22889 | 5.52391 | 69.56 | \<.0001 | 0.49624 | 2.01516 |
| 6 PM |  | 1 | 351.16156 | 5.49526 | 63.9 | \<.0001 | 0.50274 | 1.98909 |
| 7 PM |  | 1 | 241.08163 | 5.45427 | 44.2 | \<.0001 | 0.51033 | 1.95952 |
| 8 PM |  | 1 | 160.28946 | 5.43009 | 29.52 | \<.0001 | 0.51488 | 1.94219 |
| 9 PM |  | 1 | 109.83471 | 5.41135 | 20.3 | \<.0001 | 0.51845 | 1.92881 |
| 10 PM |  | 1 | 72.08962 | 5.40297 | 13.34 | \<.0001 | 0.52006 | 1.92284 |
| 11 PM |  | 1 | 32.65604 | 5.39936 | 6.05 | \<.0001 | 0.52076 | 1.92027 |
| Friday |  | 1 | 3.9631 | 2.31518 | 1.71 | 0.087 | 0.92703 | 1.07871 |
| Saturday |  | 1 | 15.65782 | 2.90975 | 5.38 | \<.0001 | 0.58202 | 1.71815 |
| Mist / Cloudy |  | 1 | \-11.826 | 1.93669 | \-6.11 | \<.0001 | 0.84128 | 1.18867 |
| Mist/Snow |  | 1 | \-68.50032 | 3.26057 | \-21.01 | \<.0001 | 0.76435 | 1.3083 |
| ln(temp) |  | 1 | 69.65795 | 2.93525 | 23.73 | \<.0001 | 0.32562 | 3.07108 |
| Humidity | Humidity | 1 | \-86.05864 | 5.56867 | \-15.45 | \<.0001 | 0.52792 | 1.89423 |
| Wind Speed | Wind Speed | 1 | \-39.6943 | 6.90482 | \-5.75 | \<.0001 | 0.85394 | 1.17105 |

SAS Code

* Day Seaonal Step Analysis

/\* 0\) open an HTML destination for all tables/plots \*/  
ods \_all\_ close;  
ods results on;  
ods html file="bike\_daily\_season\_env\_with\_atemp.html" style=HTMLBlue;  
ods graphics on;

/\* 1\) Keep target \+ seasonal \+ environmental inputs \*/  
data day\_season\_env;  
    set day(keep=cnt season mnth weathersit temp atemp hum windspeed);

    /\* \---- Season dummies (1–4); reference \= 1 \---- \*/  
    season2 \= (season \= 2);  
    season3 \= (season \= 3);  
    season4 \= (season \= 4);

    /\* \---- Month dummies (1–12); reference \= 1 \---- \*/  
    mnth2  \= (mnth \= 2);  
    mnth3  \= (mnth \= 3);  
    mnth4  \= (mnth \= 4);  
    mnth5  \= (mnth \= 5);  
    mnth6  \= (mnth \= 6);  
    mnth7  \= (mnth \= 7);  
    mnth8  \= (mnth \= 8);  
    mnth9  \= (mnth \= 9);  
    mnth10 \= (mnth \= 10);  
    mnth11 \= (mnth \= 11);  
    mnth12 \= (mnth \= 12);

    /\* \---- Weathersit dummies; reference \= 1 \---- \*/  
    weathersit2 \= (weathersit \= 2);  
    weathersit3 \= (weathersit \= 3);  
    weathersit4 \= (weathersit \= 4);

    /\* \---- Small epsilon to allow log/inverse safely \---- \*/  
    eps \= 1e-6;

    /\* \---- Transformations: TEMP \---- \*/  
    tempsq   \= temp\*\*2;  
    templog  \= log(temp \+ eps);  
    tempsqrt \= sqrt(temp);  
    tempinv  \= 1/(temp \+ eps);

    /\* \---- Transformations: ATEMP \---- \*/  
    atempsq   \= atemp\*\*2;  
    atemplog  \= log(atemp \+ eps);  
    atempsqrt \= sqrt(atemp);  
    atempinv  \= 1/(atemp \+ eps);

    /\* \---- Transformations: HUM \---- \*/  
    humsq    \= hum\*\*2;  
    humlog   \= log(hum \+ eps);  
    humsqrt  \= sqrt(hum);  
    huminv   \= 1/(hum \+ eps);

    /\* \---- Transformations: WINDSPEED \---- \*/  
    windspeedsq   \= windspeed\*\*2;  
    windspeedlog  \= log(windspeed \+ eps);  
    windspeedsqrt \= sqrt(windspeed);  
    windspeedinv  \= 1/(windspeed \+ eps);  
run;

/\* 2\) Correlation matrix \*/  
title "Correlation Matrix: Temp, Atemp, Humidity, Windspeed (Daily)";  
proc corr data=day\_season\_env nosimple plots=matrix(histogram);  
    var temp atemp templog atemplog tempsq atempsq tempsqrt atempsqrt  
        hum humsq humlog humsqrt  
        windspeed windspeedsq windspeedlog windspeedsqrt;  
run;

/\* 3\) Stepwise mode \*/  
title "Stepwise OLS: cnt \~ Season \+ Month \+ Weathersit \+ Temp/Atemp/Hum/Wind (Daily)";  
proc reg data=day\_season\_env plots=all;  
    model cnt \=  
        /\* Season \*/  
        season2 season3 season4

        /\* Month \*/  
        mnth2 mnth3 mnth4 mnth5 mnth6 mnth7 mnth8 mnth9 mnth10 mnth11 mnth12

        /\* Weathersit \*/  
        weathersit2 weathersit3 weathersit4

        /\* TEMP \+ ATEMP \+ HUM \+ WIND \+ transforms \*/  
        temp tempsq templog tempsqrt tempinv  
        atemp atempsq atemplog atempsqrt atempinv  
        hum humsq humlog humsqrt huminv  
        windspeed windspeedsq windspeedlog windspeedsqrt windspeedinv

        / selection=stepwise slentry=0.01 slstay=0.01  
          vif tol collin;  
    output out=day\_model\_out  
        r=resid  
        rstudent=rstudent  
        p=pred  
        h=leverage  
        cookd=cookd;  
run;  
quit;

/\* 4\) residual visuals \*/  
title "Residuals vs Predicted (Daily)";  
proc sgplot data=day\_model\_out;  
    scatter x=pred y=resid;  
    refline 0 / axis=y;  
run;

title "Studentized Residuals vs Predicted (Daily)";  
proc sgplot data=day\_model\_out;  
    scatter x=pred y=rstudent;  
    refline 0 / axis=y;  
run;

title "Histogram of Residuals (Daily)";  
proc sgplot data=day\_model\_out;  
    histogram resid;  
    density resid / type=kernel;  
run;

/\* 5\) Close HTML output \*/  
ods html close;  
ods results off;  
ods \_all\_ close;

* Day Seasonal Final Equaiton 

/\* 0\) Cleanly open an HTML destination for all tables/plots \*/  
ods \_all\_ close;  
ods results on;  
ods html file="bike\_daily\_season\_env\_with\_atemp.html" style=HTMLBlue;  
ods graphics on;

/\* 1\) Keep target \+ seasonal \+ environmental inputs\*/  
data day\_season\_env;  
    set day(keep=cnt season mnth weathersit temp atemp hum windspeed);

    /\* \---- Season dummies (1–4); reference \= 1 \---- \*/  
    season2 \= (season \= 2);  
    season3 \= (season \= 3);  
    season4 \= (season \= 4);

    /\* \---- Month dummies (1–12); reference \= 1 \---- \*/  
    mnth2  \= (mnth \= 2);  
    mnth3  \= (mnth \= 3);  
    mnth4  \= (mnth \= 4);  
    mnth5  \= (mnth \= 5);  
    mnth6  \= (mnth \= 6);  
    mnth7  \= (mnth \= 7);  
    mnth8  \= (mnth \= 8);  
    mnth9  \= (mnth \= 9);  
    mnth10 \= (mnth \= 10);  
    mnth11 \= (mnth \= 11);  
    mnth12 \= (mnth \= 12);

    /\* \---- Weathersit dummies; reference \= 1 \---- \*/  
    weathersit2 \= (weathersit \= 2);  
    weathersit3 \= (weathersit \= 3);  
    weathersit4 \= (weathersit \= 4);

    /\* \---- Small epsilon to allow log/inverse safely \---- \*/  
    eps \= 1e-6;

    /\* \---- Transformations: TEMP \---- \*/  
    tempsq   \= temp\*\*2;  
    templog  \= log(temp \+ eps);  
    tempsqrt \= sqrt(temp);  
    tempinv  \= 1/(temp \+ eps);

    /\* \---- Transformations: ATEMP \---- \*/  
    atempsq   \= atemp\*\*2;  
    atemplog  \= log(atemp \+ eps);  
    atempsqrt \= sqrt(atemp);  
    atempinv  \= 1/(atemp \+ eps);

    /\* \---- Transformations: HUM \---- \*/  
    humsq    \= hum\*\*2;  
    humlog   \= log(hum \+ eps);  
    humsqrt  \= sqrt(hum);  
    huminv   \= 1/(hum \+ eps);

    /\* \---- Transformations: WINDSPEED \---- \*/  
    windspeedsq   \= windspeed\*\*2;  
    windspeedlog  \= log(windspeed \+ eps);  
    windspeedsqrt \= sqrt(windspeed);  
    windspeedinv  \= 1/(windspeed \+ eps);  
run;

/\* 2\) Correlation matrix (optional) \*/  
title "Correlation Matrix: Temp, Atemp, Humidity, Windspeed (Daily)";  
proc corr data=day\_season\_env nosimple plots=matrix(histogram);  
    var temp atemp templog atemplog tempsq atempsq tempsqrt atempsqrt  
        hum humsq humlog humsqrt  
        windspeed windspeedsq windspeedlog windspeedsqrt;  
run;

/\* 3\)  Re-run final model\*/  
proc reg data=day\_season\_env plots=all;  
    model cnt \=  
        season2  
        season4  
        mnth9  
        weathersit3  
        templog  
        humsq       
        windspeed  
        / vif tol collin;  
    output out=day\_model\_out  
        r=resid  
        rstudent=rstudent  
        p=pred  
        h=leverage  
        cookd=cookd;  
run;  
quit;

/\* 4\) Optional residual visuals \*/  
title "Residuals vs Predicted (Daily)";  
proc sgplot data=day\_model\_out;  
    scatter x=pred y=resid;  
    refline 0 / axis=y;  
run;

title "Studentized Residuals vs Predicted (Daily)";  
proc sgplot data=day\_model\_out;  
    scatter x=pred y=rstudent;  
    refline 0 / axis=y;  
run;

title "Histogram of Residuals (Daily)";  
proc sgplot data=day\_model\_out;  
    histogram resid;  
    density resid / type=kernel;  
run;

/\* Residuals vs Predicted \*/  
title "Daily Seasonal+Env: Residuals vs Predicted";  
proc sgplot data=day\_model\_out;  
    scatter x=pred y=resid / transparency=0.2;  
    refline 0 / axis=y;  
    xaxis label="Predicted cnt"; yaxis label="Residuals";  
run;

/\* Actual vs Predicted with 45° line \*/  
title "Daily Seasonal+Env: Actual vs Predicted";  
proc sgplot data=day\_model\_out;  
    scatter x=pred y=cnt / transparency=0.2;  
    lineparm x=0 y=0 slope=1 / lineattrs=(pattern=shortdash);  
    xaxis label="Predicted cnt"; yaxis label="Actual cnt";  
run;

ods html close; ods graphics off; ods results off;

* Full Day Step Analysis

ods \_all\_ close;  
ods results on;  
ods graphics on;  
ods html file="bike\_daily\_full\_model.html" style=HTMLBlue;

/\* 1\) Keep variables from day dataset\*/  
data day\_full;  
    set day(keep=cnt season yr mnth holiday weekday workingday weathersit   
                   temp atemp hum windspeed);

    /\* \---- SEASON (1–4; reference \= 1\) \---- \*/  
    season2 \= (season \= 2);  
    season3 \= (season \= 3);  
    season4 \= (season \= 4);

    /\* \---- YEAR (0 \= 2011, 1 \= 2012\) — keep as-is \---- \*/  
    /\* yr already numeric dummy \*/

    /\* \---- MONTH (1–12; reference \= 1\) \---- \*/  
    mnth2  \= (mnth \= 2);  
    mnth3  \= (mnth \= 3);  
    mnth4  \= (mnth \= 4);  
    mnth5  \= (mnth \= 5);  
    mnth6  \= (mnth \= 6);  
    mnth7  \= (mnth \= 7);  
    mnth8  \= (mnth \= 8);  
    mnth9  \= (mnth \= 9);  
    mnth10 \= (mnth \= 10);  
    mnth11 \= (mnth \= 11);  
    mnth12 \= (mnth \= 12);

    /\* \---- WEEKDAY (0–6; reference \= 0/Sunday) \---- \*/  
    weekday1 \= (weekday \= 1);  
    weekday2 \= (weekday \= 2);  
    weekday3 \= (weekday \= 3);  
    weekday4 \= (weekday \= 4);  
    weekday5 \= (weekday \= 5);  
    weekday6 \= (weekday \= 6);

    /\* \---- WEATHERSIT (1–4; reference \= 1\) \---- \*/  
    weathersit2 \= (weathersit \= 2);  
    weathersit3 \= (weathersit \= 3);  
    weathersit4 \= (weathersit \= 4);

    eps \= 1e-6;

    /\* Transform temp \+ atemp \*/  
    tempsq   \= temp\*\*2;  
    templog  \= log(temp \+ eps);  
    tempsqrt \= sqrt(temp);  
    tempinv  \= 1/(temp \+ eps);

    atempsq   \= atemp\*\*2;  
    atemplog  \= log(atemp \+ eps);  
    atempsqrt \= sqrt(atemp);  
    atempinv  \= 1/(atemp \+ eps);

    /\* Transform humidity \*/  
    humsq   \= hum\*\*2;  
    humlog  \= log(hum \+ eps);  
    humsqrt \= sqrt(hum);  
    huminv  \= 1/(hum \+ eps);

    /\* Transform windspeed \*/  
    windspeedsq   \= windspeed\*\*2;  
    windspeedlog  \= log(windspeed \+ eps);  
    windspeedsqrt \= sqrt(windspeed);  
    windspeedinv  \= 1/(windspeed \+ eps);  
run;

/\* 2\) Stepwise model with ALL categorical \+ transformations \*/  
proc reg data=day\_full plots=all;  
    model cnt \=  
        yr holiday workingday  
        season2 season3 season4  
        mnth2 mnth3 mnth4 mnth5 mnth6 mnth7 mnth8 mnth9 mnth10 mnth11 mnth12  
        weekday1 weekday2 weekday3 weekday4 weekday5 weekday6  
        weathersit2 weathersit3 weathersit4  
        temp tempsq templog tempsqrt tempinv  
        atemp atempsq atemplog atempsqrt atempinv  
        hum humsq humlog humsqrt huminv  
        windspeed windspeedsq windspeedlog windspeedsqrt windspeedinv

        / selection=stepwise slentry=0.01 slstay=0.01  
          vif tol collin;

    output out=day\_full\_out  
        r=resid  
        rstudent=rstudent  
        p=pred  
        h=leverage  
        cookd=cookd;  
run;  
quit;

/\* 3\) Close ODS \*/  
ods html close;  
ods graphics off;  
ods results off;  
ods \_all\_ close;

* Full Day Final Equation

ods \_all\_ close;  
ods results on;  
ods graphics on;  
ods html file="bike\_daily\_full\_model.html" style=HTMLBlue;

/\* 1\) Keep variables from day dataset\*/  
data day\_full;  
    set day(keep=cnt season yr mnth holiday weekday workingday weathersit   
                   temp atemp hum windspeed);

    /\* \---- SEASON (1–4; reference \= 1\) \---- \*/  
    season2 \= (season \= 2);  
    season3 \= (season \= 3);  
    season4 \= (season \= 4);

    /\* \---- YEAR (0 \= 2011, 1 \= 2012\) — keep as-is \---- \*/  
    /\* yr already numeric dummy \*/

    /\* \---- MONTH (1–12; reference \= 1\) \---- \*/  
    mnth2  \= (mnth \= 2);  
    mnth3  \= (mnth \= 3);  
    mnth4  \= (mnth \= 4);  
    mnth5  \= (mnth \= 5);  
    mnth6  \= (mnth \= 6);  
    mnth7  \= (mnth \= 7);  
    mnth8  \= (mnth \= 8);  
    mnth9  \= (mnth \= 9);  
    mnth10 \= (mnth \= 10);  
    mnth11 \= (mnth \= 11);  
    mnth12 \= (mnth \= 12);

    /\* \---- WEEKDAY (0–6; reference \= 0/Sunday) \---- \*/  
    weekday1 \= (weekday \= 1);  
    weekday2 \= (weekday \= 2);  
    weekday3 \= (weekday \= 3);  
    weekday4 \= (weekday \= 4);  
    weekday5 \= (weekday \= 5);  
    weekday6 \= (weekday \= 6);

    /\* \---- WEATHERSIT (1–4; reference \= 1\) \---- \*/  
    weathersit2 \= (weathersit \= 2);  
    weathersit3 \= (weathersit \= 3);  
    weathersit4 \= (weathersit \= 4);

    eps \= 1e-6;

    /\* Transform temp \+ atemp \*/  
    tempsq   \= temp\*\*2;  
    templog  \= log(temp \+ eps);  
    tempsqrt \= sqrt(temp);  
    tempinv  \= 1/(temp \+ eps);

    atempsq   \= atemp\*\*2;  
    atemplog  \= log(atemp \+ eps);  
    atempsqrt \= sqrt(atemp);  
    atempinv  \= 1/(atemp \+ eps);

    /\* Transform humidity \*/  
    humsq   \= hum\*\*2;  
    humlog  \= log(hum \+ eps);  
    humsqrt \= sqrt(hum);  
    huminv  \= 1/(hum \+ eps);

    /\* Transform windspeed \*/  
    windspeedsq   \= windspeed\*\*2;  
    windspeedlog  \= log(windspeed \+ eps);  
    windspeedsqrt \= sqrt(windspeed);  
    windspeedinv  \= 1/(windspeed \+ eps);  
run;

/\* 2\) final model \*/

proc reg data=day\_full plots=all;  
    model cnt \=  
        /\* Time-related categorical variables \*/  
        yr  
        workingday  
        season2 season3 season4  
        mnth3                  
        weekday1 weekday6       
        weathersit2 weathersit3

        /\* Best environmental terms \*/  
        templog           
        hum                
        windspeed      

        / vif tol collin;

    output out=day\_full\_out  
        r=resid  
        rstudent=rstudent  
        p=pred  
        h=leverage  
        cookd=cookd;  
run;  
quit;

title "Daily Full: Residuals vs Predicted";  
proc sgplot data=out\_daily\_full;  
    scatter x=pred y=resid / transparency=0.2;  
    refline 0 / axis=y;  
    xaxis label="Predicted cnt"; yaxis label="Residuals";  
run;

title "Daily Full: Actual vs Predicted";  
proc sgplot data=out\_daily\_full;  
    scatter x=pred y=cnt / transparency=0.2;  
    lineparm x=0 y=0 slope=1 / lineattrs=(pattern=shortdash);  
    xaxis label="Predicted cnt"; yaxis label="Actual cnt";  
run;

/\* Residuals vs Predicted \*/  
title "Daily Seasonal+Env: Residuals vs Predicted";  
proc sgplot data=oout\_daily\_full;  
    scatter x=pred y=resid / transparency=0.2;  
    refline 0 / axis=y;  
    xaxis label="Predicted cnt"; yaxis label="Residuals";  
run;

/\* Actual vs Predicted with 45° line \*/  
title "Daily Seasonal+Env: Actual vs Predicted";  
proc sgplot data=out\_daily\_full;  
    scatter x=pred y=cnt / transparency=0.2;  
    lineparm x=0 y=0 slope=1 / lineattrs=(pattern=shortdash);  
    xaxis label="Predicted cnt"; yaxis label="Actual cnt";  
Run;

ods html close; ods graphics off; ods results off;

* Day Descriptive Statistics

ods excel file="final\_model\_summary\_stats.xlsx";

/\*new continuous\*/  
data day\_transformed;  
set day;  
    /\*original variables\*/  
       /\* TEMP transforms \*/  
    tempsq   \= temp\*\*2;  
    templog  \= log(temp \+ eps);  
    tempsqrt \= sqrt(temp);  
    tempinv  \= 1/(temp \+ eps);

    /\* ATEMP transforms (now included\!) \*/  
    atempsq   \= atemp\*\*2;  
    atemplog  \= log(atemp \+ eps);  
    atempsqrt \= sqrt(atemp);  
    atempinv  \= 1/(atemp \+ eps);

    /\* HUM transforms \*/  
    humsq    \= hum\*\*2;  
    humlog   \= log(hum \+ eps);  
    humsqrt  \= sqrt(hum);  
    huminv   \= 1/(hum \+ eps);

    /\* WIND transforms \*/  
    windspeedsq   \= windspeed\*\*2;  
    windspeedlog  \= log(windspeed \+ eps);  
    windspeedsqrt \= sqrt(windspeed);  
    windspeedinv  \= 1/(windspeed \+ eps);  
    /\*original variables\*/  
    title "Summary Statistics \- Original Daily Model";  
proc means data=day n mean std min p25 median p75 max maxdec=2;  
    var cnt temp atemp hum windspeed;  
run;

title "Summary Statistics \- Daily Seasonal \+ Environmental Final Model";  
proc means data=day\_transformed n mean std min p25 median p75 max maxdec=2;  
    var cnt templog humsq windspeed;  
run;

title "Summary Statistics \- Daily Full Model";  
proc means data=day\_transformed n mean std min p25 median p75 max maxdec=2;  
    var cnt yr workingday templog hum windspeed;  
run;  
quit;  
ods excel close;  
title "Histogram of Daily Bike Rentals";  
proc sgplot data=day;  
    histogram cnt / scale=count;  
    density cnt;  
    xaxis label="Bike Rentals Daily";  
  yaxis label="Count";  
run;  
quit;  
title "Boxplot of Daily Bike Rentals by Weather";  
proc sgplot data=day;  
    vbox cnt / category=weathersit;  
run;  
title "Boxplot of Daily Bike Rentals by Season";  
proc sgplot data=day;  
    vbox cnt / category=season;  
run;

* Day Scatterplots

Day Original   
ods html file="day" style=HTMLBlue;  
ods graphics on;

/\* cnt vs Temperature \*/  
title "Daily: Bike Count vs Temperature";  
proc sgplot data=day;  
    scatter x=temp y=cnt / transparency=0.2;  
    loess x=temp y=cnt / nomarkers;  
    xaxis label="Temperature (normalized 0–1)";  
    yaxis label="Bike Count (cnt)";  
run;

/\* cnt vs Feeling Temperature (atemp) \*/  
title "Daily: Bike Count vs Feeling Temperature (atemp)";  
proc sgplot data=day;  
    scatter x=atemp y=cnt / transparency=0.2;  
    loess x=atemp y=cnt / nomarkers;  
    xaxis label="Apparent Temperature (normalized 0–1)";  
    yaxis label="Bike Count (cnt)";  
run;

/\* cnt vs Humidity \*/  
title "Daily: Bike Count vs Humidity";  
proc sgplot data=day;  
    scatter x=hum y=cnt / transparency=0.2;  
    loess x=hum y=cnt / nomarkers;  
    xaxis label="Humidity (0–1)";  
    yaxis label="Bike Count (cnt)";  
run;

/\* cnt vs Windspeed \*/  
title "Daily: Bike Count vs Windspeed";  
proc sgplot data=day;  
    scatter x=windspeed y=cnt / transparency=0.2;  
    loess x=windspeed y=cnt / nomarkers;  
    xaxis label="Windspeed (0–1)";  
    yaxis label="Bike Count (cnt)";  
run;

ods html close;  
ods graphics off;

Day Seaonal

ods html file="bike\_daily\_scatterplots.html" style=HTMLBlue;  
ods graphics on;  
/\* 1\) Keep target \+ seasonal \+ environmental inputs\*/  
data day\_season\_env;  
    set day(keep=cnt season mnth weathersit temp atemp hum windspeed);

    /\* \---- Season dummies (1–4); reference \= 1 \---- \*/  
    season2 \= (season \= 2);  
    season3 \= (season \= 3);  
    season4 \= (season \= 4);

    /\* \---- Month dummies (1–12); reference \= 1 \---- \*/  
    mnth2  \= (mnth \= 2);  
    mnth3  \= (mnth \= 3);  
    mnth4  \= (mnth \= 4);  
    mnth5  \= (mnth \= 5);  
    mnth6  \= (mnth \= 6);  
    mnth7  \= (mnth \= 7);  
    mnth8  \= (mnth \= 8);  
    mnth9  \= (mnth \= 9);  
    mnth10 \= (mnth \= 10);  
    mnth11 \= (mnth \= 11);  
    mnth12 \= (mnth \= 12);

    /\* \---- Weathersit dummies; reference \= 1 \---- \*/  
    weathersit2 \= (weathersit \= 2);  
    weathersit3 \= (weathersit \= 3);  
    weathersit4 \= (weathersit \= 4);

    /\* \---- Small epsilon to allow log/inverse safely \---- \*/  
    eps \= 1e-6;

    /\* \---- Transformations: TEMP \---- \*/  
    tempsq   \= temp\*\*2;  
    templog  \= log(temp \+ eps);  
    tempsqrt \= sqrt(temp);  
    tempinv  \= 1/(temp \+ eps);

    /\* \---- Transformations: ATEMP \---- \*/  
    atempsq   \= atemp\*\*2;  
    atemplog  \= log(atemp \+ eps);  
    atempsqrt \= sqrt(atemp);  
    atempinv  \= 1/(atemp \+ eps);

    /\* \---- Transformations: HUM \---- \*/  
    humsq    \= hum\*\*2;  
    humlog   \= log(hum \+ eps);  
    humsqrt  \= sqrt(hum);  
    huminv   \= 1/(hum \+ eps);

    /\* \---- Transformations: WINDSPEED \---- \*/  
    windspeedsq   \= windspeed\*\*2;  
    windspeedlog  \= log(windspeed \+ eps);  
    windspeedsqrt \= sqrt(windspeed);  
    windspeedinv  \= 1/(windspeed \+ eps);  
run;

/\* cnt vs Spring (season2) \*/  
title "Bike Count vs Spring (season2)";  
proc sgplot data=day\_season\_env;  
    scatter x=season2 y=cnt / transparency=0.2;  
    loess x=season2 y=cnt / nomarkers;  
    xaxis label="Spring (1 \= Spring, 0 \= Other Seasons)";  
    yaxis label="Bike Count (cnt)";  
run;

/\* cnt vs Winter (season4) \*/  
title "Bike Count vs Winter (season4)";  
proc sgplot data=day\_season\_env;  
    scatter x=season4 y=cnt / transparency=0.2;  
    loess x=season4 y=cnt / nomarkers;  
    xaxis label="Winter (1 \= Winter, 0 \= Other Seasons)";  
    yaxis label="Bike Count (cnt)";  
run;

/\* cnt vs September (mnth9) \*/  
title "Bike Count vs September (mnth9)";  
proc sgplot data=day\_season\_env;  
    scatter x=mnth9 y=cnt / transparency=0.2;  
    loess x=mnth9 y=cnt / nomarkers;  
    xaxis label="September (1 \= September, 0 \= Other Months)";  
    yaxis label="Bike Count (cnt)";  
run;

/\* cnt vs Weather (weathersit3) \*/  
title "Bike Count vs Snow/Fog Conditions (weathersit3)";  
proc sgplot data=day\_season\_env;  
    scatter x=weathersit3 y=cnt / transparency=0.2;  
    loess x=weathersit3 y=cnt / nomarkers;  
    xaxis label="Weather: Light Snow or Heavy Fog (1 \= Yes, 0 \= No)";  
    yaxis label="Bike Count (cnt)";  
run;

/\* cnt vs log(Temperature) \*/  
title "Bike Count vs log(Temperature)";  
proc sgplot data=day\_season\_env;  
    scatter x=templog y=cnt / transparency=0.2;  
    loess x=templog y=cnt / nomarkers;  
    xaxis label="log(Temperature)";  
    yaxis label="Bike Count (cnt)";  
run;

/\* cnt vs Humidity^2 \*/  
title "Bike Count vs Humidity Squared (humsq)";  
proc sgplot data=day\_season\_env;  
    scatter x=humsq y=cnt / transparency=0.2;  
    loess x=humsq y=cnt / nomarkers;  
    xaxis label="Humidity²";  
    yaxis label="Bike Count (cnt)";  
run;

/\* cnt vs Windspeed \*/  
title "Bike Count vs Windspeed";  
proc sgplot data=day\_season\_env;  
    scatter x=windspeed y=cnt / transparency=0.2;  
    loess x=windspeed y=cnt / nomarkers;  
    xaxis label="Windspeed (0–1 normalized)";  
    yaxis label="Bike Count (cnt)";  
run;

ods html close;  
ods graphics off;

Day full

ods \_all\_ close;  
ods results on;  
ods graphics on;  
ods html file="bike\_daily\_full\_model.html" style=HTMLBlue;

/\* 1\) Keep variables from day dataset\*/  
data day\_full;  
    set day(keep=cnt season yr mnth holiday weekday workingday weathersit   
                   temp atemp hum windspeed);

    /\* \---- SEASON (1–4; reference \= 1\) \---- \*/  
    season2 \= (season \= 2);  
    season3 \= (season \= 3);  
    season4 \= (season \= 4);

    /\* \---- YEAR (0 \= 2011, 1 \= 2012\) — keep as-is \---- \*/  
    /\* yr already numeric dummy \*/

    /\* \---- MONTH (1–12; reference \= 1\) \---- \*/  
    mnth2  \= (mnth \= 2);  
    mnth3  \= (mnth \= 3);  
    mnth4  \= (mnth \= 4);  
    mnth5  \= (mnth \= 5);  
    mnth6  \= (mnth \= 6);  
    mnth7  \= (mnth \= 7);  
    mnth8  \= (mnth \= 8);  
    mnth9  \= (mnth \= 9);  
    mnth10 \= (mnth \= 10);  
    mnth11 \= (mnth \= 11);  
    mnth12 \= (mnth \= 12);

    /\* \---- WEEKDAY (0–6; reference \= 0/Sunday) \---- \*/  
    weekday1 \= (weekday \= 1);  
    weekday2 \= (weekday \= 2);  
    weekday3 \= (weekday \= 3);  
    weekday4 \= (weekday \= 4);  
    weekday5 \= (weekday \= 5);  
    weekday6 \= (weekday \= 6);

    /\* \---- WEATHERSIT (1–4; reference \= 1\) \---- \*/  
    weathersit2 \= (weathersit \= 2);  
    weathersit3 \= (weathersit \= 3);  
    weathersit4 \= (weathersit \= 4);

    eps \= 1e-6;

    /\* Transform temp \+ atemp \*/  
    tempsq   \= temp\*\*2;  
    templog  \= log(temp \+ eps);  
    tempsqrt \= sqrt(temp);  
    tempinv  \= 1/(temp \+ eps);

    atempsq   \= atemp\*\*2;  
    atemplog  \= log(atemp \+ eps);  
    atempsqrt \= sqrt(atemp);  
    atempinv  \= 1/(atemp \+ eps);

    /\* Transform humidity \*/  
    humsq   \= hum\*\*2;  
    humlog  \= log(hum \+ eps);  
    humsqrt \= sqrt(hum);  
    huminv  \= 1/(hum \+ eps);

    /\* Transform windspeed \*/  
    windspeedsq   \= windspeed\*\*2;  
    windspeedlog  \= log(windspeed \+ eps);  
    windspeedsqrt \= sqrt(windspeed);  
    windspeedinv  \= 1/(windspeed \+ eps);  
run;

/\* Binary/indicator predictors \*/  
title "cnt vs Year 2012 (yr)";  
proc sgplot data=day\_full; scatter x=yr y=cnt / transparency=0.2; xaxis label="Year 2012 (1/0)"; yaxis label="cnt"; run;

title "cnt vs Working Day";  
proc sgplot data=day\_full; scatter x=workingday y=cnt / transparency=0.2; xaxis label="Working Day (1/0)"; yaxis label="cnt"; run;

title "cnt vs Spring (season2)";  
proc sgplot data=day\_full; scatter x=season2 y=cnt / transparency=0.2; xaxis label="Spring (1/0)"; yaxis label="cnt"; run;

title "cnt vs Summer (season3)";  
proc sgplot data=day\_full; scatter x=season3 y=cnt / transparency=0.2; xaxis label="Summer (1/0)"; yaxis label="cnt"; run;

title "cnt vs Winter (season4)";  
proc sgplot data=day\_full; scatter x=season4 y=cnt / transparency=0.2; xaxis label="Winter (1/0)"; yaxis label="cnt"; run;

title "cnt vs March (mnth3)";  
proc sgplot data=day\_full; scatter x=mnth3 y=cnt / transparency=0.2; xaxis label="March (1/0)"; yaxis label="cnt"; run;

title "cnt vs Monday (weekday1)";  
proc sgplot data=day\_full; scatter x=weekday1 y=cnt / transparency=0.2; xaxis label="Monday (1/0)"; yaxis label="cnt"; run;

title "cnt vs Saturday (weekday6)";  
proc sgplot data=day\_full; scatter x=weekday6 y=cnt / transparency=0.2; xaxis label="Saturday (1/0)"; yaxis label="cnt"; run;

title "cnt vs Mist/Cloudy (weathersit2)";  
proc sgplot data=day\_full; scatter x=weathersit2 y=cnt / transparency=0.2; xaxis label="Mist/Cloudy (1/0)"; yaxis label="cnt"; run;

title "cnt vs Snow/Heavy Mist (weathersit3)";  
proc sgplot data=day\_full; scatter x=weathersit3 y=cnt / transparency=0.2; xaxis label="Snow or Heavy Mist (1/0)"; yaxis label="cnt"; run;

/\* Continuous predictors (add LOESS) \*/  
title "cnt vs log(Temperature) (templog)";  
proc sgplot data=day\_full;  
    scatter x=templog y=cnt / transparency=0.2;  
    loess   x=templog y=cnt / nomarkers;  
    xaxis label="log(Temperature)"; yaxis label="cnt";  
run;

title "cnt vs Humidity (hum)";  
proc sgplot data=day\_full;  
    scatter x=hum y=cnt / transparency=0.2;  
    loess   x=hum y=cnt / nomarkers;  
    xaxis label="Humidity (0–1)"; yaxis label="cnt";  
run;

title "cnt vs Windspeed";  
proc sgplot data=day\_full;  
    scatter x=windspeed y=cnt / transparency=0.2;  
    loess   x=windspeed y=cnt / nomarkers;  
    xaxis label="Windspeed (0–1)"; yaxis label="cnt";  
run;

ods html close; ods graphics off;

Hour Original  
ods html file="scatter\_hour\_originals.html" style=HTMLBlue;  
ods graphics on;

/\* cnt vs Temperature \*/  
title "Hourly: Bike Count vs Temperature";  
proc sgplot data=hour;  
    scatter x=temp y=cnt / transparency=0.2;  
    loess x=temp y=cnt / nomarkers;  
    xaxis label="Temperature (normalized 0–1)";  
    yaxis label="Bike Count (cnt)";  
run;

/\* cnt vs Feeling Temperature (atemp) \*/  
title "Hourly: Bike Count vs Feeling Temperature (atemp)";  
proc sgplot data=hour;  
    scatter x=atemp y=cnt / transparency=0.2;  
    loess x=atemp y=cnt / nomarkers;  
    xaxis label="Apparent Temperature (normalized 0–1)";  
    yaxis label="Bike Count (cnt)";  
run;

/\* cnt vs Humidity \*/  
title "Hourly: Bike Count vs Humidity";  
proc sgplot data=hour;  
    scatter x=hum y=cnt / transparency=0.2;  
    loess x=hum y=cnt / nomarkers;  
    xaxis label="Humidity (0–1)";  
    yaxis label="Bike Count (cnt)";  
run;

/\* cnt vs Windspeed \*/  
title "Hourly: Bike Count vs Windspeed";  
proc sgplot data=hour;  
    scatter x=windspeed y=cnt / transparency=0.2;  
    loess x=windspeed y=cnt / nomarkers;  
    xaxis label="Windspeed (0–1)";  
    yaxis label="Bike Count (cnt)";  
run;

ods html close;  
ods graphics off;

* Hour Seasonal Step Analysis

/\* 0\) Cleanly open ODS HTML for output \*/  
ods \_all\_ close;  
ods results on;  
ods html file="bike\_hourly\_season\_env\_with\_atemp.html" style=HTMLBlue;  
ods graphics on;

/\* 1\) Keep target \+ seasonal \+ environmental predictors \*/  
data hour\_season\_env;  
    set hour(keep=cnt season mnth weathersit temp atemp hum windspeed);

    /\* Season dummies (reference \= 1\) \*/  
    season2 \= (season \= 2);  
    season3 \= (season \= 3);  
    season4 \= (season \= 4);

    /\* Month dummies (reference \= 1\) \*/  
    mnth2  \= (mnth \= 2);  
    mnth3  \= (mnth \= 3);  
    mnth4  \= (mnth \= 4);  
    mnth5  \= (mnth \= 5);  
    mnth6  \= (mnth \= 6);  
    mnth7  \= (mnth \= 7);  
    mnth8  \= (mnth \= 8);  
    mnth9  \= (mnth \= 9);  
    mnth10 \= (mnth \= 10);  
    mnth11 \= (mnth \= 11);  
    mnth12 \= (mnth \= 12);

    /\* Weathersit dummies (reference \= 1\) \*/  
    weathersit2 \= (weathersit \= 2);  
    weathersit3 \= (weathersit \= 3);  
    weathersit4 \= (weathersit \= 4);

    /\* Safe epsilon for transformations \*/  
    eps \= 1e-6;

    /\* TEMP transforms \*/  
    tempsq   \= temp\*\*2;  
    templog  \= log(temp \+ eps);  
    tempsqrt \= sqrt(temp);  
    tempinv  \= 1/(temp \+ eps);

    /\* ATEMP transforms (now included\!) \*/  
    atempsq   \= atemp\*\*2;  
    atemplog  \= log(atemp \+ eps);  
    atempsqrt \= sqrt(atemp);  
    atempinv  \= 1/(atemp \+ eps);

    /\* HUM transforms \*/  
    humsq    \= hum\*\*2;  
    humlog   \= log(hum \+ eps);  
    humsqrt  \= sqrt(hum);  
    huminv   \= 1/(hum \+ eps);

    /\* WIND transforms \*/  
    windspeedsq   \= windspeed\*\*2;  
    windspeedlog  \= log(windspeed \+ eps);  
    windspeedsqrt \= sqrt(windspeed);  
    windspeedinv  \= 1/(windspeed \+ eps);  
run;

/\* 2\) Optional correlation check \*/  
title "Correlation Matrix: Temp, Atemp, Humidity, Windspeed (Hourly)";  
proc corr data=hour\_season\_env nosimple plots=matrix(histogram);  
    var temp atemp templog atemplog tempsq atempsq tempsqrt atempsqrt  
        hum humsq humlog humsqrt  
        windspeed windspeedsq windspeedlog windspeedsqrt;  
run;

/\* 3\) Stepwise regression including ATEMP \+ transforms \*/  
title "Stepwise OLS: Hourly cnt \~ Season \+ Month \+ Weathersit \+ Temp/Atemp/Hum/Wind";  
proc reg data=hour\_season\_env plots=all;  
    model cnt \=  
        /\* Season \*/  
        season2 season3 season4  
        /\* Month \*/  
        mnth2 mnth3 mnth4 mnth5 mnth6 mnth7 mnth8 mnth9 mnth10 mnth11 mnth12  
        /\* Weather categories \*/  
        weathersit2 weathersit3 weathersit4

        /\* Environmental variables: temp \+ atemp \+ hum \+ windspeed \*/  
        temp tempsq templog tempsqrt tempinv  
        atemp atempsq atemplog atempsqrt atempinv  
        hum humsq humlog humsqrt huminv  
        windspeed windspeedsq windspeedlog windspeedsqrt windspeedinv

        / selection=stepwise slentry=0.01 slstay=0.01  
          vif tol collin;

    output out=hour\_model\_out  
        r=resid  
        rstudent=rstudent  
        p=pred  
        h=leverage  
        cookd=cookd;  
run;  
quit;

/\* 4\) Optional residual visuals \*/  
title "Residuals vs Predicted (Hourly)";  
proc sgplot data=hour\_model\_out;  
    scatter x=pred y=resid;  
    refline 0 / axis=y;  
run;

title "Studentized Residuals vs Predicted (Hourly)";  
proc sgplot data=hour\_model\_out;  
    scatter x=pred y=rstudent;  
    refline 0 / axis=y;  
run;

title "Histogram of Residuals (Hourly)";  
proc sgplot data=hour\_model\_out;  
    histogram resid;  
    density resid / type=kernel;  
run;

/\* 5\) Close HTML output \*/  
ods html close;  
ods results off;  
ods \_all\_ close;

* Hour Seasonal Final Equation

/\* 0\) Cleanly open ODS HTML for output \*/  
ods \_all\_ close;  
ods results on;  
ods html file="bike\_hourly\_season\_env\_with\_atemp.html" style=HTMLBlue;  
ods graphics on;

/\* 1\) \*/  
data hour\_season\_env;  
    set hour(keep=cnt season mnth weathersit temp atemp hum windspeed);

    /\* Season dummies (reference \= 1\) \*/  
    season2 \= (season \= 2);  
    season3 \= (season \= 3);  
    season4 \= (season \= 4);

    /\* Month dummies (reference \= 1\) \*/  
    mnth2  \= (mnth \= 2);  
    mnth3  \= (mnth \= 3);  
    mnth4  \= (mnth \= 4);  
    mnth5  \= (mnth \= 5);  
    mnth6  \= (mnth \= 6);  
    mnth7  \= (mnth \= 7);  
    mnth8  \= (mnth \= 8);  
    mnth9  \= (mnth \= 9);  
    mnth10 \= (mnth \= 10);  
    mnth11 \= (mnth \= 11);  
    mnth12 \= (mnth \= 12);

    /\* Weathersit dummies (reference \= 1\) \*/  
    weathersit2 \= (weathersit \= 2);  
    weathersit3 \= (weathersit \= 3);  
    weathersit4 \= (weathersit \= 4);

    /\* Safe epsilon for transformations \*/  
    eps \= 1e-6;

    /\* TEMP transforms \*/  
    tempsq   \= temp\*\*2;  
    templog  \= log(temp \+ eps);  
    tempsqrt \= sqrt(temp);  
    tempinv  \= 1/(temp \+ eps);

    /\* ATEMP transforms (now included\!) \*/  
    atempsq   \= atemp\*\*2;  
    atemplog  \= log(atemp \+ eps);  
    atempsqrt \= sqrt(atemp);  
    atempinv  \= 1/(atemp \+ eps);

    /\* HUM transforms \*/  
    humsq    \= hum\*\*2;  
    humlog   \= log(hum \+ eps);  
    humsqrt  \= sqrt(hum);  
    huminv   \= 1/(hum \+ eps);

    /\* WIND transforms \*/  
    windspeedsq   \= windspeed\*\*2;  
    windspeedlog  \= log(windspeed \+ eps);  
    windspeedsqrt \= sqrt(windspeed);  
    windspeedinv  \= 1/(windspeed \+ eps);  
run;

/\* 2\) correlation check \*/  
title "Correlation Matrix: Temp, Atemp, Humidity, Windspeed (Hourly)";  
proc corr data=hour\_season\_env nosimple plots=matrix(histogram);  
    var temp atemp templog atemplog tempsq atempsq tempsqrt atempsqrt  
        hum humsq humlog humsqrt  
        windspeed windspeedsq windspeedlog windspeedsqrt;  
run;

/\* 3\) Final Hourly Seasonal \+ Environmental Regression Model \*/

proc reg data=hour\_season\_env plots=all;  
    model cnt \=  
        /\* Season  \*/  
        season2 season4

        /\* Months \*/  
        mnth4 mnth5 mnth6 mnth7 mnth8 mnth9 mnth12

        /\* Weather situation \*/  
        weathersit2

        /\* Environmental \*/  
        templog       
        hum          /  
        windspeed    

        / vif tol collin;

    output out=hour\_model\_out  
        r=resid  
        rstudent=rstudent  
        p=pred  
        h=leverage  
        cookd=cookd;  
run;  
quit;

/\* 4\) Optional residual visuals \*/  
title "Residuals vs Predicted (Hourly)";  
proc sgplot data=hour\_model\_out;  
    scatter x=pred y=resid;  
    refline 0 / axis=y;  
run;

title "Studentized Residuals vs Predicted (Hourly)";  
proc sgplot data=hour\_model\_out;  
    scatter x=pred y=rstudent;  
    refline 0 / axis=y;  
run;

title "Histogram of Residuals (Hourly)";  
proc sgplot data=hour\_model\_out;  
    histogram resid;  
    density resid / type=kernel;  
run;

title "Hourly Seasonal+Env: Residuals vs Predicted";  
proc sgplot data=hour\_model\_out;  
    scatter x=pred y=resid / transparency=0.2;  
    refline 0 / axis=y;  
    xaxis label="Predicted cnt"; yaxis label="Residuals";  
run;

title "Hourly Seasonal+Env: Actual vs Predicted";  
proc sgplot data=hour\_model\_out;  
    scatter x=pred y=cnt / transparency=0.2;  
    lineparm x=0 y=0 slope=1 / lineattrs=(pattern=shortdash);  
    xaxis label="Predicted cnt"; yaxis label="Actual cnt";  
run;

title "Hourly Seasonal+Env: Residuals vs Predicted";  
proc sgplot data=hour\_model\_out;  
    scatter x=pred y=resid / transparency=0.2;  
    refline 0 / axis=y;  
    xaxis label="Predicted cnt"; yaxis label="Residuals";  
run;

title "Hourly Seasonal+Env: Actual vs Predicted";  
proc sgplot data=hour\_model\_out;  
    scatter x=pred y=cnt / transparency=0.2;  
    lineparm x=0 y=0 slope=1 / lineattrs=(pattern=shortdash);  
    xaxis label="Predicted cnt"; yaxis label="Actual cnt";  
run;

ods html close; ods graphics off; ods results off;

* Full Hourly Step Analysis

ods \_all\_ close;  
ods results on;  
ods graphics on;  
ods html file="bike\_hourly\_full\_model.html" style=HTMLBlue;

/\* 1\) Prepare dataset with ALL predictors (no arrays) \*/  
data hour\_full;  
    set hour(keep=cnt season yr mnth hr holiday weekday workingday  
                  weathersit temp atemp hum windspeed);

    /\* \------- Season Dummies (1=Ref) \------- \*/  
    season2 \= (season \= 2);  
    season3 \= (season \= 3);  
    season4 \= (season \= 4);

    /\* \------- Month Dummies \------- \*/  
    mnth2  \= (mnth \= 2);  
    mnth3  \= (mnth \= 3);  
    mnth4  \= (mnth \= 4);  
    mnth5  \= (mnth \= 5);  
    mnth6  \= (mnth \= 6);  
    mnth7  \= (mnth \= 7);  
    mnth8  \= (mnth \= 8);  
    mnth9  \= (mnth \= 9);  
    mnth10 \= (mnth \= 10);  
    mnth11 \= (mnth \= 11);  
    mnth12 \= (mnth \= 12);

    /\* \------- Hour Dummies (0-23 → 0 is reference) \------- \*/  
    hr1 \= (hr \= 1);   hr2 \= (hr \= 2);   hr3 \= (hr \= 3);   hr4 \= (hr \= 4);  
    hr5 \= (hr \= 5);   hr6 \= (hr \= 6);   hr7 \= (hr \= 7);   hr8 \= (hr \= 8);  
    hr9 \= (hr \= 9);   hr10 \= (hr \= 10); hr11 \= (hr \= 11); hr12 \= (hr \= 12);  
    hr13 \= (hr \= 13); hr14 \= (hr \= 14); hr15 \= (hr \= 15); hr16 \= (hr \= 16);  
    hr17 \= (hr \= 17); hr18 \= (hr \= 18); hr19 \= (hr \= 19); hr20 \= (hr \= 20);  
    hr21 \= (hr \= 21); hr22 \= (hr \= 22); hr23 \= (hr \= 23);

    /\* \------- Weekday (0 \= Sunday ref) \------- \*/  
    weekday1 \= (weekday \= 1);  
    weekday2 \= (weekday \= 2);  
    weekday3 \= (weekday \= 3);  
    weekday4 \= (weekday \= 4);  
    weekday5 \= (weekday \= 5);  
    weekday6 \= (weekday \= 6);

    /\* \------- Weathersit \------- \*/  
    weathersit2 \= (weathersit \= 2);  
    weathersit3 \= (weathersit \= 3);  
    weathersit4 \= (weathersit \= 4);

    /\* \------- Transformations for continuous vars \------- \*/  
    eps \= 1e-6;

    /\* temp \*/  
    tempsq   \= temp\*\*2;  
    templog  \= log(temp \+ eps);  
    tempsqrt \= sqrt(temp);  
    tempinv  \= 1/(temp \+ eps);

    /\* atemp \*/  
    atempsq   \= atemp\*\*2;  
    atemplog  \= log(atemp \+ eps);  
    atempsqrt \= sqrt(atemp);  
    atempinv  \= 1/(atemp \+ eps);

    /\* humidity \*/  
    humsq   \= hum\*\*2;  
    humlog  \= log(hum \+ eps);  
    humsqrt \= sqrt(hum);  
    huminv  \= 1/(hum \+ eps);

    /\* windspeed \*/  
    windspeedsq   \= windspeed\*\*2;  
    windspeedlog  \= log(windspeed \+ eps);  
    windspeedsqrt \= sqrt(windspeed);  
    windspeedinv  \= 1/(windspeed \+ eps);  
run;

/\* 2\) Full Stepwise Regression \*/  
title "Hourly Full Stepwise Model – All Variables & Transforms";  
proc reg data=hour\_full plots=all;  
    model cnt \=  
        yr holiday workingday  
        season2 season3 season4  
        mnth2 mnth3 mnth4 mnth5 mnth6 mnth7 mnth8 mnth9 mnth10 mnth11 mnth12  
        hr1 hr2 hr3 hr4 hr5 hr6 hr7 hr8 hr9 hr10 hr11 hr12  
        hr13 hr14 hr15 hr16 hr17 hr18 hr19 hr20 hr21 hr22 hr23  
        weekday1 weekday2 weekday3 weekday4 weekday5 weekday6  
        weathersit2 weathersit3 weathersit4  
        temp tempsq templog tempsqrt tempinv  
        atemp atempsq atemplog atempsqrt atempinv  
        hum humsq humlog humsqrt huminv  
        windspeed windspeedsq windspeedlog windspeedsqrt windspeedinv

        / selection=stepwise slentry=0.01 slstay=0.01  
          vif tol collin;

    output out=hour\_full\_out  
        r=resid rstudent=rstudent p=pred cookd=cookd h=leverage;

    output out=hour\_model\_out  
        r=resid  
        rstudent=rstudent  
        p=pred  
        h=leverage  
        cookd=cookd;  
run;  
quit;

/\* 4\) Optional residual visuals \*/  
title "Residuals vs Predicted (Hourly)";  
proc sgplot data=hour\_model\_out;  
    scatter x=pred y=resid;  
    refline 0 / axis=y;  
run;

title "Studentized Residuals vs Predicted (Hourly)";  
proc sgplot data=hour\_model\_out;  
    scatter x=pred y=rstudent;  
    refline 0 / axis=y;  
run;

title "Histogram of Residuals (Hourly)";  
proc sgplot data=hour\_model\_out;  
    histogram resid;  
    density resid / type=kernel;  
run;

/\* 5\) Close HTML output \*/  
ods html close;  
ods results off;  
ods \_all\_ close;

* Full Hourly Final Equation

ods \_all\_ close;  
ods results on;  
ods graphics on;  
ods html file="bike\_hourly\_full\_model.html" style=HTMLBlue;

/\* 1\) Prepare dataset with ALL predictors \*/  
data hour\_full;  
    set hour(keep=cnt season yr mnth hr holiday weekday workingday  
                  weathersit temp atemp hum windspeed);

    /\* \------- Season Dummies (1=Ref) \------- \*/  
    season2 \= (season \= 2);  
    season3 \= (season \= 3);  
    season4 \= (season \= 4);

    /\* \------- Month Dummies \------- \*/  
    mnth2  \= (mnth \= 2);  
    mnth3  \= (mnth \= 3);  
    mnth4  \= (mnth \= 4);  
    mnth5  \= (mnth \= 5);  
    mnth6  \= (mnth \= 6);  
    mnth7  \= (mnth \= 7);  
    mnth8  \= (mnth \= 8);  
    mnth9  \= (mnth \= 9);  
    mnth10 \= (mnth \= 10);  
    mnth11 \= (mnth \= 11);  
    mnth12 \= (mnth \= 12);

    /\* \------- Hour Dummies (0-23 → 0 is reference) \------- \*/  
    hr1 \= (hr \= 1);   hr2 \= (hr \= 2);   hr3 \= (hr \= 3);   hr4 \= (hr \= 4);  
    hr5 \= (hr \= 5);   hr6 \= (hr \= 6);   hr7 \= (hr \= 7);   hr8 \= (hr \= 8);  
    hr9 \= (hr \= 9);   hr10 \= (hr \= 10); hr11 \= (hr \= 11); hr12 \= (hr \= 12);  
    hr13 \= (hr \= 13); hr14 \= (hr \= 14); hr15 \= (hr \= 15); hr16 \= (hr \= 16);  
    hr17 \= (hr \= 17); hr18 \= (hr \= 18); hr19 \= (hr \= 19); hr20 \= (hr \= 20);  
    hr21 \= (hr \= 21); hr22 \= (hr \= 22); hr23 \= (hr \= 23);

    /\* \------- Weekday (0 \= Sunday ref) \------- \*/  
    weekday1 \= (weekday \= 1);  
    weekday2 \= (weekday \= 2);  
    weekday3 \= (weekday \= 3);  
    weekday4 \= (weekday \= 4);  
    weekday5 \= (weekday \= 5);  
    weekday6 \= (weekday \= 6);

    /\* \------- Weathersit \------- \*/  
    weathersit2 \= (weathersit \= 2);  
    weathersit3 \= (weathersit \= 3);  
    weathersit4 \= (weathersit \= 4);

    /\* \------- Transformations for continuous vars \------- \*/  
    eps \= 1e-6;

    /\* temp \*/  
    tempsq   \= temp\*\*2;  
    templog  \= log(temp \+ eps);  
    tempsqrt \= sqrt(temp);  
    tempinv  \= 1/(temp \+ eps);

    /\* atemp \*/  
    atempsq   \= atemp\*\*2;  
    atemplog  \= log(atemp \+ eps);  
    atempsqrt \= sqrt(atemp);  
    atempinv  \= 1/(atemp \+ eps);

    /\* humidity \*/  
    humsq   \= hum\*\*2;  
    humlog  \= log(hum \+ eps);  
    humsqrt \= sqrt(hum);  
    huminv  \= 1/(hum \+ eps);

    /\* windspeed \*/  
    windspeedsq   \= windspeed\*\*2;  
    windspeedlog  \= log(windspeed \+ eps);  
    windspeedsqrt \= sqrt(windspeed);  
    windspeedinv  \= 1/(windspeed \+ eps);  
run;

/\* 2\) Final Clean Hourly Model \*/

proc reg data=hour\_full plots=all;  
    model cnt \=  
        /\* \--- Time Effects \--- \*/  
        yr holiday workingday  
        season2 season3 season4  
        mnth2 mnth3 mnth6 mnth7 mnth8 mnth9 mnth10  
        /\* (Keep only months SAS selected — adjust based on your output) \*/

        /\* Hour of Day\*/  
        hr1 hr2 hr3 hr4 hr5 hr6 hr7 hr8 hr9 hr10 hr11 hr12  
        hr13 hr14 hr15 hr16 hr17 hr18 hr19 hr20 hr21 hr22 hr23

        /\* Weekdays kept \*/  
        weekday5 weekday6  /\* based on output screenshot \*/

        /\* Weather \*/  
        weathersit2 weathersit3

        /\* \--- Environmental Predictors (Clean) \--- \*/  
        templog           
        hum               
        windspeed       

        / vif tol collin;

    output out=hour\_model\_out  
        r=resid  
        rstudent=rstudent  
        p=pred  
        h=leverage  
        cookd=cookd;  
run;  
quit;

/\* 4\) Optional residual visuals \*/  
title "Residuals vs Predicted (Hourly)";  
proc sgplot data=hour\_model\_out;  
    scatter x=pred y=resid;  
    refline 0 / axis=y;  
run;

title "Studentized Residuals vs Predicted (Hourly)";  
proc sgplot data=hour\_model\_out;  
    scatter x=pred y=rstudent;  
    refline 0 / axis=y;  
run;

title "Histogram of Residuals (Hourly)";  
proc sgplot data=hour\_model\_out;  
    histogram resid;  
    density resid / type=kernel;  
run;

title "Hourly Full: Residuals vs Predicted";  
proc sgplot data=hour\_model\_out;  
    scatter x=pred y=resid / transparency=0.2;  
    refline 0 / axis=y;  
    xaxis label="Predicted cnt"; yaxis label="Residuals";  
run;

title "Hourly Full: Actual vs Predicted";  
proc sgplot data=hour\_model\_out;  
    scatter x=pred y=cnt / transparency=0.2;  
    lineparm x=0 y=0 slope=1 / lineattrs=(pattern=shortdash);  
    xaxis label="Predicted cnt"; yaxis label="Actual cnt";  
run;

ods html close; ods graphics off; ods results off;

* Hour Descriptive Statistics 

ods excel file="final\_model\_summary\_stats.xlsx";

/\*new continuous\*/  
data hour\_transformed;  
set hour;  
    /\*original variables\*/  
       /\* TEMP transforms \*/  
    tempsq   \= temp\*\*2;  
    templog  \= log(temp \+ eps);  
    tempsqrt \= sqrt(temp);  
    tempinv  \= 1/(temp \+ eps);

    /\* ATEMP transforms (now included\!) \*/  
    atempsq   \= atemp\*\*2;  
    atemplog  \= log(atemp \+ eps);  
    atempsqrt \= sqrt(atemp);  
    atempinv  \= 1/(atemp \+ eps);

    /\* HUM transforms \*/  
    humsq    \= hum\*\*2;  
    humlog   \= log(hum \+ eps);  
    humsqrt  \= sqrt(hum);  
    huminv   \= 1/(hum \+ eps);

    /\* WIND transforms \*/  
    windspeedsq   \= windspeed\*\*2;  
    windspeedlog  \= log(windspeed \+ eps);  
    windspeedsqrt \= sqrt(windspeed);  
    windspeedinv  \= 1/(windspeed \+ eps);

    title "Summary Statistics \- Original Hourly Model";  
proc means data=hour n mean std min p25 median p75 max maxdec=2;  
    var cnt temp atemp hum windspeed;  
run;

title "Hourly Seasonal \+ Environmental";  
proc means data=hour\_transformed n mean std min p25 median p75 max maxdec=2;  
    var cnt templog hum windspeed;  
run;

title "Hourly Full Model";  
proc means data=hour\_transformed n mean std min p25 median p75 max maxdec=2;  
    var cnt yr holiday workingday templog hum windspeed;  
run;  
quit;  
ods excel close;  
title "Histogram of Hourly Bike Rentals";  
proc sgplot data=hour;  
    histogram cnt / scale=count;  
    density cnt;  
    xaxis label="Bike Rentals Hourly";  
  yaxis label="Count";  
run;  
quit;  
title "Boxplot of Hourly Bike Rentals by Weather";  
proc sgplot data=hour;  
    vbox cnt / category=weathersit;  
run;  
title "Boxplot of Hourly Bike Rentals by Season";  
proc sgplot data=hour;  
    vbox cnt / category=season;  
run;

* Hour Scatterplots

Hour Seasonal  
/\* 1\) Prepare dataset with ALL predictors \*/  
data hour\_season\_env;  
    set hour(keep=cnt season yr mnth hr holiday weekday workingday  
                  weathersit temp atemp hum windspeed);

    /\* \------- Season Dummies (1=Ref) \------- \*/  
    season2 \= (season \= 2);  
    season3 \= (season \= 3);  
    season4 \= (season \= 4);

    /\* \------- Month Dummies \------- \*/  
    mnth2  \= (mnth \= 2);  
    mnth3  \= (mnth \= 3);  
    mnth4  \= (mnth \= 4);  
    mnth5  \= (mnth \= 5);  
    mnth6  \= (mnth \= 6);  
    mnth7  \= (mnth \= 7);  
    mnth8  \= (mnth \= 8);  
    mnth9  \= (mnth \= 9);  
    mnth10 \= (mnth \= 10);  
    mnth11 \= (mnth \= 11);  
    mnth12 \= (mnth \= 12);

    /\* \------- Hour Dummies (0-23 → 0 is reference) \------- \*/  
    hr1 \= (hr \= 1);   hr2 \= (hr \= 2);   hr3 \= (hr \= 3);   hr4 \= (hr \= 4);  
    hr5 \= (hr \= 5);   hr6 \= (hr \= 6);   hr7 \= (hr \= 7);   hr8 \= (hr \= 8);  
    hr9 \= (hr \= 9);   hr10 \= (hr \= 10); hr11 \= (hr \= 11); hr12 \= (hr \= 12);  
    hr13 \= (hr \= 13); hr14 \= (hr \= 14); hr15 \= (hr \= 15); hr16 \= (hr \= 16);  
    hr17 \= (hr \= 17); hr18 \= (hr \= 18); hr19 \= (hr \= 19); hr20 \= (hr \= 20);  
    hr21 \= (hr \= 21); hr22 \= (hr \= 22); hr23 \= (hr \= 23);

    /\* \------- Weekday (0 \= Sunday ref) \------- \*/  
    weekday1 \= (weekday \= 1);  
    weekday2 \= (weekday \= 2);  
    weekday3 \= (weekday \= 3);  
    weekday4 \= (weekday \= 4);  
    weekday5 \= (weekday \= 5);  
    weekday6 \= (weekday \= 6);

    /\* \------- Weathersit \------- \*/  
    weathersit2 \= (weathersit \= 2);  
    weathersit3 \= (weathersit \= 3);  
    weathersit4 \= (weathersit \= 4);

    /\* \------- Transformations for continuous vars \------- \*/  
    eps \= 1e-6;

    /\* temp \*/  
    tempsq   \= temp\*\*2;  
    templog  \= log(temp \+ eps);  
    tempsqrt \= sqrt(temp);  
    tempinv  \= 1/(temp \+ eps);

    /\* atemp \*/  
    atempsq   \= atemp\*\*2;  
    atemplog  \= log(atemp \+ eps);  
    atempsqrt \= sqrt(atemp);  
    atempinv  \= 1/(atemp \+ eps);

    /\* humidity \*/  
    humsq   \= hum\*\*2;  
    humlog  \= log(hum \+ eps);  
    humsqrt \= sqrt(hum);  
    huminv  \= 1/(hum \+ eps);

    /\* windspeed \*/  
    windspeedsq   \= windspeed\*\*2;  
    windspeedlog  \= log(windspeed \+ eps);  
    windspeedsqrt \= sqrt(windspeed);  
    windspeedinv  \= 1/(windspeed \+ eps);  
run;

ods html file="scatter\_hour\_season\_env.html" style=HTMLBlue; ods graphics on;

/\* Seasonal/month/weather indicators \*/  
title "cnt vs Spring (season2)";  
proc sgplot data=hour\_season\_env; scatter x=season2 y=cnt / transparency=0.2; xaxis label="Spring (1/0)"; yaxis label="cnt"; run;

title "cnt vs Winter (season4)";  
proc sgplot data=hour\_season\_env; scatter x=season4 y=cnt / transparency=0.2; xaxis label="Winter (1/0)"; yaxis label="cnt"; run;

title "cnt vs April (mnth4)";  
proc sgplot data=hour\_season\_env; scatter x=mnth4 y=cnt / transparency=0.2; xaxis label="April (1/0)"; yaxis label="cnt"; run;

title "cnt vs May (mnth5)";  
proc sgplot data=hour\_season\_env; scatter x=mnth5 y=cnt / transparency=0.2; xaxis label="May (1/0)"; yaxis label="cnt"; run;

title "cnt vs June (mnth6)";  
proc sgplot data=hour\_season\_env; scatter x=mnth6 y=cnt / transparency=0.2; xaxis label="June (1/0)"; yaxis label="cnt"; run;

title "cnt vs July (mnth7)";  
proc sgplot data=hour\_season\_env; scatter x=mnth7 y=cnt / transparency=0.2; xaxis label="July (1/0)"; yaxis label="cnt"; run;

title "cnt vs August (mnth8)";  
proc sgplot data=hour\_season\_env; scatter x=mnth8 y=cnt / transparency=0.2; xaxis label="August (1/0)"; yaxis label="cnt"; run;

title "cnt vs September (mnth9)";  
proc sgplot data=hour\_season\_env; scatter x=mnth9 y=cnt / transparency=0.2; xaxis label="September (1/0)"; yaxis label="cnt"; run;

title "cnt vs December (mnth12)";  
proc sgplot data=hour\_season\_env; scatter x=mnth12 y=cnt / transparency=0.2; xaxis label="December (1/0)"; yaxis label="cnt"; run;

title "cnt vs Mist/Cloudy (weathersit2)";  
proc sgplot data=hour\_season\_env; scatter x=weathersit2 y=cnt / transparency=0.2; xaxis label="Mist/Cloudy (1/0)"; yaxis label="cnt"; run;

/\* Continuous \*/  
title "cnt vs log(Temperature) (templog)";  
proc sgplot data=hour\_season\_env;  
    scatter x=templog y=cnt / transparency=0.2;  
    loess   x=templog y=cnt / nomarkers;  
    xaxis label="log(Temperature)"; yaxis label="cnt";  
run;

title "cnt vs Humidity (hum)";  
proc sgplot data=hour\_season\_env;  
    scatter x=hum y=cnt / transparency=0.2;  
    loess   x=hum y=cnt / nomarkers;  
    xaxis label="Humidity (0–1)"; yaxis label="cnt";  
run;

title "cnt vs Windspeed";  
proc sgplot data=hour\_season\_env;  
    scatter x=windspeed y=cnt / transparency=0.2;  
    loess   x=windspeed y=cnt / nomarkers;  
    xaxis label="Windspeed (0–1)"; yaxis label="cnt";  
run;

ods html close; ods graphics off;

Hour full  
/\* 1\) Prepare dataset with ALL predictors \*/  
data hour\_full;  
    set hour(keep=cnt season yr mnth hr holiday weekday workingday  
                  weathersit temp atemp hum windspeed);

    /\* \------- Season Dummies (1=Ref) \------- \*/  
    season2 \= (season \= 2);  
    season3 \= (season \= 3);  
    season4 \= (season \= 4);

    /\* \------- Month Dummies \------- \*/  
    mnth2  \= (mnth \= 2);  
    mnth3  \= (mnth \= 3);  
    mnth4  \= (mnth \= 4);  
    mnth5  \= (mnth \= 5);  
    mnth6  \= (mnth \= 6);  
    mnth7  \= (mnth \= 7);  
    mnth8  \= (mnth \= 8);  
    mnth9  \= (mnth \= 9);  
    mnth10 \= (mnth \= 10);  
    mnth11 \= (mnth \= 11);  
    mnth12 \= (mnth \= 12);

    /\* \------- Hour Dummies (0-23 → 0 is reference) \------- \*/  
    hr1 \= (hr \= 1);   hr2 \= (hr \= 2);   hr3 \= (hr \= 3);   hr4 \= (hr \= 4);  
    hr5 \= (hr \= 5);   hr6 \= (hr \= 6);   hr7 \= (hr \= 7);   hr8 \= (hr \= 8);  
    hr9 \= (hr \= 9);   hr10 \= (hr \= 10); hr11 \= (hr \= 11); hr12 \= (hr \= 12);  
    hr13 \= (hr \= 13); hr14 \= (hr \= 14); hr15 \= (hr \= 15); hr16 \= (hr \= 16);  
    hr17 \= (hr \= 17); hr18 \= (hr \= 18); hr19 \= (hr \= 19); hr20 \= (hr \= 20);  
    hr21 \= (hr \= 21); hr22 \= (hr \= 22); hr23 \= (hr \= 23);

    /\* \------- Weekday (0 \= Sunday ref) \------- \*/  
    weekday1 \= (weekday \= 1);  
    weekday2 \= (weekday \= 2);  
    weekday3 \= (weekday \= 3);  
    weekday4 \= (weekday \= 4);  
    weekday5 \= (weekday \= 5);  
    weekday6 \= (weekday \= 6);

    /\* \------- Weathersit \------- \*/  
    weathersit2 \= (weathersit \= 2);  
    weathersit3 \= (weathersit \= 3);  
    weathersit4 \= (weathersit \= 4);

    /\* \------- Transformations for continuous vars \------- \*/  
    eps \= 1e-6;

    /\* temp \*/  
    tempsq   \= temp\*\*2;  
    templog  \= log(temp \+ eps);  
    tempsqrt \= sqrt(temp);  
    tempinv  \= 1/(temp \+ eps);

    /\* atemp \*/  
    atempsq   \= atemp\*\*2;  
    atemplog  \= log(atemp \+ eps);  
    atempsqrt \= sqrt(atemp);  
    atempinv  \= 1/(atemp \+ eps);

    /\* humidity \*/  
    humsq   \= hum\*\*2;  
    humlog  \= log(hum \+ eps);  
    humsqrt \= sqrt(hum);  
    huminv  \= 1/(hum \+ eps);

    /\* windspeed \*/  
    windspeedsq   \= windspeed\*\*2;  
    windspeedlog  \= log(windspeed \+ eps);  
    windspeedsqrt \= sqrt(windspeed);  
    windspeedinv  \= 1/(windspeed \+ eps);  
run;

ods html file="scatter\_hour\_full.html" style=HTMLBlue; ods graphics on;

/\* Year / calendar flags \*/  
title "cnt vs Year 2012 (yr)";  
proc sgplot data=hour\_full; scatter x=yr y=cnt / transparency=0.2; xaxis label="Year 2012 (1/0)"; yaxis label="cnt"; run;

title "cnt vs Holiday";  
proc sgplot data=hour\_full; scatter x=holiday y=cnt / transparency=0.2; xaxis label="Holiday (1/0)"; yaxis label="cnt"; run;

title "cnt vs Working Day";  
proc sgplot data=hour\_full; scatter x=workingday y=cnt / transparency=0.2; xaxis label="Working Day (1/0)"; yaxis label="cnt"; run;

/\* Seasons \*/  
title "cnt vs Spring (season2)";  
proc sgplot data=hour\_full; scatter x=season2 y=cnt / transparency=0.2; xaxis label="Spring (1/0)"; yaxis label="cnt"; run;

title "cnt vs Summer (season3)";  
proc sgplot data=hour\_full; scatter x=season3 y=cnt / transparency=0.2; xaxis label="Summer (1/0)"; yaxis label="cnt"; run;

title "cnt vs Winter (season4)";  
proc sgplot data=hour\_full; scatter x=season4 y=cnt / transparency=0.2; xaxis label="Winter (1/0)"; yaxis label="cnt"; run;

/\* Months kept in final model \*/  
title "cnt vs February (mnth2)";  
proc sgplot data=hour\_full; scatter x=mnth2 y=cnt / transparency=0.2; xaxis label="February (1/0)"; yaxis label="cnt"; run;

title "cnt vs June (mnth6)";  
proc sgplot data=hour\_full; scatter x=mnth6 y=cnt / transparency=0.2; xaxis label="June (1/0)"; yaxis label="cnt"; run;

title "cnt vs July (mnth7)";  
proc sgplot data=hour\_full; scatter x=mnth7 y=cnt / transparency=0.2; xaxis label="July (1/0)"; yaxis label="cnt"; run;

title "cnt vs August (mnth8)";  
proc sgplot data=hour\_full; scatter x=mnth8 y=cnt / transparency=0.2; xaxis label="August (1/0)"; yaxis label="cnt"; run;

title "cnt vs September (mnth9)";  
proc sgplot data=hour\_full; scatter x=mnth9 y=cnt / transparency=0.2; xaxis label="September (1/0)"; yaxis label="cnt"; run;

title "cnt vs October (mnth10)";  
proc sgplot data=hour\_full; scatter x=mnth10 y=cnt / transparency=0.2; xaxis label="October (1/0)"; yaxis label="cnt"; run;

/\* Hour-of-day effects (hr1–hr23 retained) \*/  
title "cnt vs 1 AM (hr1)";  proc sgplot data=hour\_full; scatter x=hr1  y=cnt / transparency=0.2; xaxis label="1 AM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 2 AM (hr2)";  proc sgplot data=hour\_full; scatter x=hr2  y=cnt / transparency=0.2; xaxis label="2 AM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 3 AM (hr3)";  proc sgplot data=hour\_full; scatter x=hr3  y=cnt / transparency=0.2; xaxis label="3 AM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 4 AM (hr4)";  proc sgplot data=hour\_full; scatter x=hr4  y=cnt / transparency=0.2; xaxis label="4 AM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 5 AM (hr5)";  proc sgplot data=hour\_full; scatter x=hr5  y=cnt / transparency=0.2; xaxis label="5 AM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 6 AM (hr6)";  proc sgplot data=hour\_full; scatter x=hr6  y=cnt / transparency=0.2; xaxis label="6 AM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 7 AM (hr7)";  proc sgplot data=hour\_full; scatter x=hr7  y=cnt / transparency=0.2; xaxis label="7 AM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 8 AM (hr8)";  proc sgplot data=hour\_full; scatter x=hr8  y=cnt / transparency=0.2; xaxis label="8 AM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 9 AM (hr9)";  proc sgplot data=hour\_full; scatter x=hr9  y=cnt / transparency=0.2; xaxis label="9 AM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 10 AM (hr10)"; proc sgplot data=hour\_full; scatter x=hr10 y=cnt / transparency=0.2; xaxis label="10 AM (1/0)"; yaxis label="cnt"; run;  
title "cnt vs 11 AM (hr11)"; proc sgplot data=hour\_full; scatter x=hr11 y=cnt / transparency=0.2; xaxis label="11 AM (1/0)"; yaxis label="cnt"; run;  
title "cnt vs 12 PM (hr12)"; proc sgplot data=hour\_full; scatter x=hr12 y=cnt / transparency=0.2; xaxis label="12 PM (1/0)"; yaxis label="cnt"; run;  
title "cnt vs 1 PM (hr13)";  proc sgplot data=hour\_full; scatter x=hr13 y=cnt / transparency=0.2; xaxis label="1 PM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 2 PM (hr14)";  proc sgplot data=hour\_full; scatter x=hr14 y=cnt / transparency=0.2; xaxis label="2 PM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 3 PM (hr15)";  proc sgplot data=hour\_full; scatter x=hr15 y=cnt / transparency=0.2; xaxis label="3 PM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 4 PM (hr16)";  proc sgplot data=hour\_full; scatter x=hr16 y=cnt / transparency=0.2; xaxis label="4 PM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 5 PM (hr17)";  proc sgplot data=hour\_full; scatter x=hr17 y=cnt / transparency=0.2; xaxis label="5 PM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 6 PM (hr18)";  proc sgplot data=hour\_full; scatter x=hr18 y=cnt / transparency=0.2; xaxis label="6 PM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 7 PM (hr19)";  proc sgplot data=hour\_full; scatter x=hr19 y=cnt / transparency=0.2; xaxis label="7 PM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 8 PM (hr20)";  proc sgplot data=hour\_full; scatter x=hr20 y=cnt / transparency=0.2; xaxis label="8 PM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 9 PM (hr21)";  proc sgplot data=hour\_full; scatter x=hr21 y=cnt / transparency=0.2; xaxis label="9 PM (1/0)";  yaxis label="cnt"; run;  
title "cnt vs 10 PM (hr22)"; proc sgplot data=hour\_full; scatter x=hr22 y=cnt / transparency=0.2; xaxis label="10 PM (1/0)"; yaxis label="cnt"; run;  
title "cnt vs 11 PM (hr23)"; proc sgplot data=hour\_full; scatter x=hr23 y=cnt / transparency=0.2; xaxis label="11 PM (1/0)"; yaxis label="cnt"; run;

/\* Weekdays present in final model \*/  
title "cnt vs Friday (weekday5)";  
proc sgplot data=hour\_full; scatter x=weekday5 y=cnt / transparency=0.2; xaxis label="Friday (1/0)"; yaxis label="cnt"; run;

title "cnt vs Saturday (weekday6)";  
proc sgplot data=hour\_full; scatter x=weekday6 y=cnt / transparency=0.2; xaxis label="Saturday (1/0)"; yaxis label="cnt"; run;

/\* Weather flags \*/  
title "cnt vs Mist/Cloudy (weathersit2)";  
proc sgplot data=hour\_full; scatter x=weathersit2 y=cnt / transparency=0.2; xaxis label="Mist/Cloudy (1/0)"; yaxis label="cnt"; run;

title "cnt vs Snow/Heavy Mist (weathersit3)";  
proc sgplot data=hour\_full; scatter x=weathersit3 y=cnt / transparency=0.2; xaxis label="Snow or Heavy Mist (1/0)"; yaxis label="cnt"; run;

/\* Continuous \*/  
title "cnt vs log(Temperature) (templog)";  
proc sgplot data=hour\_full;  
    scatter x=templog y=cnt / transparency=0.2;  
    loess   x=templog y=cnt / nomarkers;  
    xaxis label="log(Temperature)"; yaxis label="cnt";  
run;

title "cnt vs Humidity (hum)";  
proc sgplot data=hour\_full;  
    scatter x=hum y=cnt / transparency=0.2;  
    loess   x=hum y=cnt / nomarkers;  
    xaxis label="Humidity (0–1)"; yaxis label="cnt";  
run;

title "cnt vs Windspeed";  
proc sgplot data=hour\_full;  
    scatter x=windspeed y=cnt / transparency=0.2;  
    loess   x=windspeed y=cnt / nomarkers;  
    xaxis label="Windspeed (0–1)"; yaxis label="cnt";  
run;

ods html close; ods graphics off;  


