# King County House Sales Linear Regression

### Underlying Question

How accurately can we predict residential housing sales and what are the most important features that drive higher prices?

### The Dataset

For this project, the dataset contains house sale prices for King County between May 2014 and May 2015.  The scrubbing process including recasting columns into their proper types, handling null values and placeholder values, creating categorical variables, and feature engineering new ones.

### EDA

![bars](https://i.imgur.com/BeKwXBm.png)

Looking at the prices (y-axis) across all the different bedroom options, we can immediately see that certain features are strongly correlated with house prices.  Notably, when a house has a condition below 3, the sales price has an upper limit much lower than that of other houses.  Likewise, the same goes for grade - when a house has a grade below 7, the average price stays almost flat, even if there are more bedrooms. Grade seems to have much more of an impact insofar as, even with bigger houses, the average price does not increase when the grade is below 7.  Finally, we can see that houses that were built or renovated since 2000 have a slightly higher average selling price.


### Feature Engineering - Zip Codes & Average Price / Frequency

Location is incredibly important when buying or selling a house - it determines schooling, transportation, and work options, and, in Manhattan, for example, different neighborhoods have totally different cultures and price ranges. 

![zip code](https://i.imgur.com/IYOlCTP.pngg)

Plotting the longitude and latitude of our dataset and coloring houses by price quartile, we can see that the bottom 25% houses are, generally, clustered together and away from the top 25%.  Generally, one can assume that similar houses in a single neighborhood have similar selling prices. But when compared to similar houses in different neighborhoods (like one's that have a higher crime rate, wealthier neighborhoods), they don't really compare.

Additionally, when buying or selling a house, members of both parties are often interested in price/sqft - as different neighborhoods can have different ranges (like we see in New York City) - and you can compare prices of houses in the same neighborhood.

And, right now, we have all these houses being considered the same way, so instead of trying to map out zip codes:

1. Add a column of Price/SqFt to our data.
2. Group houses by zip code, and find the mean price per square feet for our zip codes
3. Split zip codes into three types of "neighborhoods" or "groups"
	- Zip codes that have sold below the average price (cheaper areas)
	- Zipcodes that sell above the average price, but not often (don't want to have their values overrepresented)
	- Zip codes that sell above the average price, and more frequently.
4. Categorize these groupings as "cheap", "expensive low volume", and "expensive high volume"
5. With this, I can drop zip codes from our dataset, and ignore longitude and latitude, as that should also be encapsulated in the zip code pricing groupings

#### Comparison of original scatterplot of Square Feet vs. Price & scatterplot colored by zip code type: 

![sq](https://i.imgur.com/3BbUQhU.png)


We can see that the different zip code groups fall into distinct and different price ranges. We can also see that houses in each group span the full range of square feet options, and the ones in the cheaper zip codes sell for noticeably less.  We can also see that the low volume, more expensive houses have, generally, higher prices than the high volume, more expensive houses - so it's a good thing we split up our data this way so that the low volume houses don't inflate our price projections unfairly.


## Linear Regression Model

### Accuracy Results

**Train Mean Squared Error**: 21068364374.01

**Test Mean Squared Error**: 18476939233.65

**R Squared**: 0.736

**Mean Absolute Error**: 97162.01

**Root Mean Squared Error**: 135929.91

**Average Predicted Price**: $506,256.66

**Average Actual Price**: $505,905.38

**Difference**: 351.27

After training a preliminary model with our feature variables, we used stepwise feature selection to eliminate feature variables that had little to no impact on model performance.  We were able to drop the number of feature variables from 53 to 19 - though, that number is still inflated given that many are categorical.

Our model looks pretty accurate, with an R Squared of 0.736, which is a measure of how close the data is to the fitted regression line. A score of 0.736 means that around ~74% of the variability can be explained by our model, but this could have been driven up by the number of features used. The Root Mean Squared Error of 135,929 is a measure of how far from the regression line data points are (on average), so our model is about 136,000 off of actual house values. Not much has changed here.

### Model Feature Variables

So, for the continuous variables, we used:

- Square Feet Living
- Square Feet Living (15)
- Square Feet Lot
- Square Feet Lot (15)

And for the categorical variables, we used:
- Grade, Condition
- View, Waterfront
- Bedrooms, Bathrooms, Floors
- Recently Renovated/Built (feature engineered)
- Zip Code Type (feature engineered)