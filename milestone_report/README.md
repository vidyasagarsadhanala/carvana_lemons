## 1st Capstone Project

# Predicting Lemon titles (Kicks) in Car Auctions

#### Vidyasagar Sadhanala


![Home]( https://github.com/vidyasagarsadhanala/carvana_lemons/blob/master/images/carvana-header.png)

## Objective:
To predict if the car purchased at the auction is a good/bad buy among thousands of cars purchased through online auctions. The goal is to create a machine learning model to predict the condition of the vehicle being purchased at a car auction, if it is a good/bad buy, hence reducing the risk.  

## Problem:
Predict if the car being purchased at auction is Good or Bad buy?

## Outcome:
One of the challenges for an auto dealership in purchasing a used car at an auction is the risk of that vehicle might have serious issues that prevent it from being resold. These are referred to as “kicks” or unfortunate purchases and are often resulting in a significant loss. Some examples of kicks could be tampered odometers, mechanical issues the dealer is not able to address, issues with getting the vehicle title from the seller or some unforeseen problem. Using machine learning to predict which cars have higher risk can provide real value to dealerships as they can predict kicks before the dealership buys at auctions.

## Dataset:
Source: https://www.kaggle.com/c/DontGetKicked/data
![Kaggle](https://github.com/vidyasagarsadhanala/carvana_lemons/blob/master/images/kaggle_intro.PNG)

Train set – 60%
Test set – 40%

The data set contains information about each car, like purchase price, make and model, trim level, odometer reading, date of purchase, state of origin and so on. There are about 40 different variables (along with the lemon status indicator IsBadBuy) on around 72K cars, the test data set has the same information on around 40K cars. The target variable is “IsBadBuy” which is a binary variable and is a post-purchase classification for kicked on non-kicked cars.

## Evaluation Metrics:
The evaluation metrics for this problem are going to be the Gini Index, Classification Accuracy %, F1 Score, Precision, Recall.


## Data Wrangling
First, we input data & do data wrangling. We conduct the following steps to clean up data and pick useful features.

1. Select columns that might be useful
 * Visually inspect variables & select the target variable and observing the different data types in the data set (numerical 19 and categorical 15).
2. Analyze further & remove unwanted columns
 * Originally there are 34 features in the dataset, then further analyzed some of columns that have high number of missing values and removed some unwanted columns.
 * For example, PRIMEUNIT and AUCGUART features have only 3419 values out of 72983.
 * Plotted the histograms for all the features to visualize their frequencies.
 * Used a python package called pandas_profiling to provide a detailed report for profiling the dataset.
3. Drop duplicates
 * Dropped duplicate features such as WheelTypeID which same as the WheelType feature, however, first one is code and second feature is a description.
 * Dropped some of the ID columns such as BYRNO and RefID
4. Fix Categorical Features
 * Then looking through all the features. The categorical features are encoded using sklearn LabelEncoder to encode then for further usage.
 * Replace some data with special characters for engine characteristics like I 4, I-4 are same as I4.
 * Replace dates with datetime object and transformed them into numerical data by extracting Purch Month, Day and Year.
5. Fix missing values
 * Plot matrix plot from the missingno package for columns with missing values and inspect the trend.
 * Confirmed that most of the features are either missing at random. 
 * Fill missing values with mean for most features.
![Missing](https://github.com/vidyasagarsadhanala/carvana_lemons/blob/master/images/missing.PNG)

## Notes based on analysis
* The target variable IsBadBuy a binary classification variable, meaning we are assigned a value of 1 if the car purchased in a Bad buy or 0 if a car is not a Bad buy (good buy).
* It is important to note that while doing this prediction we need to be careful about the excessive cost of predicting false negatives. This means that a dealership might think that this car is a good buy and think they would be able to sell it, however in reality this a Bad Buy and not sellable.
* A false positive has a cost associated with it as well, if the purchase as classified as a Bad buy it is indeed a sellable car, then the dealership might lose the opportunity selling the used car and generating profit of it.

## EDA Summary
* The dataset does contain missing data values, but all features are of the correct data type.
* The strongest positive correlations with the target features are: VehicleAge, VehOdo, WarrantyCost, VNZIP1.
* The strongest negative correlations with the target features are: VehYear, MMRAcquisitionAuctionAveragePrice, MMRCurrentAuctionAveragePrice, and MMRCurrentAuctionCleanPrice.
* The dataset is imbalanced with the majoriy of observations describing as Good Buys.
* Few features are redundant for our analysis, namely: RefId, WheelTypeID, PurchDate, and VNST.


## Initial Findings
During exploratory data analysis, analyzed following questions:

1. Does Vehicle Age has a different distribution for Good buy vs Bad Buys?
- The Vehicle Age by the target variable IsBadBuy seems to follow the same distribution as the non-bad buy purchases.
![Question1](https://github.com/vidyasagarsadhanala/carvana_lemons/blob/master/images/q1.PNG)
 
2. Does the Vehicle being sold online impact Good vs Bad Buys?
- The percentage of vehicles selling online seems to be low across Good and Bad buys in this data set.
- IsOnlineSale = 11.5%
- Not IsOnlinebSale = 12.3%

3. What is the mean vehicle age for Good vs Bad Buys?
- The mean vehicle age seems to higher for bad buys than compared to good buys.
- Avg Vehcile Age for Non Bad Buy: 4.1
- Avg Vehcile Age for Bad Buy: 4.9

4. Is the WarrantyCost high for Bad Buys?
- The distribution of the WarrantyCost (showing KDE) seems to be lower for Bad Buys than Good Buys.
![Question4](https://github.com/vidyasagarsadhanala/carvana_lemons/blob/master/images/q4.PNG)

## Hypothesis Testing
Based on the Warranty Costs for Good buys and Bad Buys, seen above, formulated the hypothesis below.
- Null Hypothesis (H0) 			: WarrantyCost are no different between BadBuys and Good Buys
- Alternative Hypothesis (HA) 	: WarrantyCosts are differnt between BadBuys and GoodBuys

* Performed a T-test from scipy stats package to observe the difference is means for the two sames (Bad Buys and Good Buys).
* The observed p-values are less than 0.05 threshold and we can reject the null hypothesis that the Warranty Costs for Bad Buys and Good Good Buys are similar.

