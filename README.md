# Used-Cars
Analysis of factors driving used car prices and targeted recommendations to used car dealers

We use the CRISP-DM framework as a basis for analysis of the factors driving used car prices. 

Summary of findings: The results of our findings demonstrate that model, age ("year"), and a bit less importantly, number of miles on a car's odometer, are key ingredients in determining a used car price. More specifically, the best models can be as much as $22k more valuable than the worse models. A new car with very little usage can be worth as much as $30k more than an older car, with more variance at higher prices.

There are other important factors. The secondary factors that we recommend used car dealers consider when evaluating inventory are: # of cylinders, condition of the car and whether "4wd" is there. This is the high level summary, but we break it down in depth for our used car dealer clients.

For car "model" and "year", which we have determined as the most important factors, we look at some of the specific ranges of values we might expect. On the high end of "model", a Nissan 370z Nismo Coupe 2d averages 40,000 dollars. The Chrysler PT Cruiser averages 3,537 dollars, on the other hand. So, the dispersion in prices using an averaging calculation called "James-Stein" is approximately 36.5k. However, that is a simplistic way of thinking about it. What if the more expensive models tend to be in better condition, or newer, on average? What if there is less mileage, on average? Luckily, our regression model helps us distill these components into a single number. The coefficient on our model, 22,695, represents the difference in expected value between that most expensive model and the least expensive model, ALL ELSE EQUAL. So, that Let us make this more concrete: the average price of a Mini Cooper S, used, is 8,729 dollars. The average price of a Jeep Wrangler is $17,749. That is a difference of 10,020 dollars. If I have a Mini Cooper S in inventory that is woth 10,000, and I were to see a Jeep Wrangler with similar features (age, odometer, condition, etc.), I would expect the Jeep to be worth 10,000 + 10,020 * 22695 / 36500 = 16,230 dollars. That kind of analysis is precise, and very helpful to used car dealers!

Let us move on to "year." Unfortunately, the intuition is more complex for our model of the relationship between "year" (age) value and price, since we have a polynomial fit of degree 2. In more simple terms, when the age of a car goes from new to 2 years old, the price goes down, and continues going down, but at some point, that actually changes. Very old cars stop going down in price as much, because they are already cheap, and very, very old cars may even have some antique value. Now, this may also depend on the model specifics, and that is a level of complexity our analysis does not go into.

What about secondary variables, like "odometer", "cylinders", "drive", and "condition"? We go through the effect on price in our model, 1 by 1.

a) Cylinder - We predict that having 8 cylinders adds 3,800 in value to a car and a 4 cylinder car is worth 2,700 less than a baseline of a 6 cylinder car.

b) Odometer - We predict that a car with 150,000 miles on it is worth $3,500 less than a car with 50,000 miles on it. The "coefficient" is -0.035 on the model. Now, there are cars with 500,000 miles on them, and clearly the model would predict a significant loss in value there, but those cars are few and far between, and for most cars, the "odometer" factor has only a moderate influence. Why? Because age ("year") and "condition" are similar factors that play a role and diminish the effect of the "odometer" on car value. In simple English, it's the condition of the car, the age of the car, AND the mileage of the car!

c) Drive - The variable that seems to matter here is whether a car is "4wd" or not. "4wd" cars command a premium of $2,550 vs other drive types

d) Condition - A car in "new" condition should be worth as much as 8,000 more than a car in "salvage" condition. So, why is this category not more important? The vast majority of cars are in the middle - "good" or "excellent" - and the next most populated category is "like new." So, an "excellent" car should be worth $1,640 more than a "good" car, which is the important takeaway here.

Data used: used car dataset from Kaggle, vehicles.csv
Key assumptions made: SFS plus LinearRegression, along with key transformations of features (converting year to polynomial of degree 2 for example) will be a sufficient modelling approach, and reducing the dataset substantially by getting rid of certain rows with NAs, rather than imputing the values, will still produce robust restults
Data selected and filtered: VIN, ID dropped due to irrelevance. Size dropped due to large number of NAs. Region and State dropped due to large number of categorical values, small sample size for many values, and risk of overfitting. Outliers removed for "Price" and "Odometer".  Categorical features converted to numbers via OHE and ordinal encoding. 
Model selections: Sequential Feature Selection with 7 or 8 features, using LinearRegression. OHE and ordinal encoding for categorical features. Min / max scaling for numerical features. Final model had no scaling for odometer, which might be suboptimal relative to StandardScaler()



