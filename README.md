# Machine Learning For Institutional Advanced Order Recognition & Market Reaction Opportunities
![Infographic](https://github.com/carrizom/darkpooltrading/blob/master/infographic.png)
## Introduction
Institutional financial firms use specialized orders and deep industry liquidity to fill asset trades at preferable rates and high volume. In this project, we will focus on dark-pool block trades, market-maker (MM) preferred trades, and hide-not-slide orders. All of these trading mechanisms are allowed and encouraged to an extent by central exchanges and regulators through lax regulation and skimpy enforcement of clearing laws due to technical loopholes in (international) trade execution venues. For instance, firms can delay reporting of billion-dollar trades for up to 48 hours by routing orders from New York to a British trade desk and back to the states for clearing. These practices, combined with other particular orders that bypass book liquidity (hide-not-slide, pegged, etc.) enable firms to conceal their market intentions and transact at away-from-market prices not available to the general public at the time of reporting. We intend to measure these sorts of trades and use machine learning to implement 0+ HFT and options-swing-trading algorithms to profit by mimicking "smart" investments and other specialized market opportunities based on hidden institutional trades.

## Methods
For dark pool trading, we created a dataset using daily data from Bezinga Pro where transactions with more than 500,000 shares were sold within the month of October. We had five main features that we analyzed within 1,492 data points. These features were: number of shares sold, time of sale, price per unit, closing price of stock (for the day of the trade), and percent difference between unit price and closing price. 

**Data Preprocessing.**
Benzinga Pro provided us with the number of shares sold, time of sale, and price per unit data in csv format. To attain the additional features and analyze the resulting data we cleaned and pre-processed the data as follows: 

- Obtained closing prices for each stock on the day that the block trade occurred  

- Removed trades/data points that did not have closing prices available for that day 

 - Calculated percent difference between unit price and closing price of each remaining stock 
To help understand the raw data we created histograms and/or box plots for each individual feature. Since our stock prices have a lot of variance in terms of magnitude, the price difference was normalized between -1 and 1. Through these graphs we observed that the time a significant amount of block trades occur is around 3pm to 4pm. While price difference had the largest grouping of data from 0.00 - 0.50. There was a huge range of number of shares sold, but we realized that most of the transactions were concentrated in the 0-500k range. Likewise in the unit price, were most of the prices were between $0-$50. We think by getting rid of some of these outliers, we might be able to get a way better model in the supervised learning phase.

<img src="https://github.com/carrizom/darkpooltrading/blob/master/Screen%20Shot%202020-11-04%20at%204.52.44%20PM.png" alt="time" width="450"/><img src="https://github.com/carrizom/darkpooltrading/blob/master/Screen%20Shot%202020-11-04%20at%204.52.34%20PM.png" alt="price diff" width="450"/>

<img src="https://github.com/carrizom/darkpooltrading/blob/master/Screen%20Shot%202020-11-06%20at%207.35.27%20PM.png" alt="unit price" width="450"/><img src="https://github.com/carrizom/darkpooltrading/blob/master/Screen%20Shot%202020-11-06%20at%207.27.36%20PM.png" alt="size of order" width="450"/>

**Feature Analysis.**
To further our understanding of each feature and its relationship with the others we created pairwise plots between combinations of two features at a time which are more comprehensive raw data visual representations of the heatmap below. Most of our data is not very linearly correlated outside of unit price and closing price. Based on the pairwise plots and heat map, the largest feature correlation we see—outside of feature vs itself and unit price vs close price—is between the normalized difference values and shares sold with a -.18 inverse correlation which is not notably strong.   
<img src="https://github.com/carrizom/darkpooltrading/blob/master/Screen%20Shot%202020-11-04%20at%204.52.25%20PM.png" alt="heatmap" width="450"/><img src="https://github.com/carrizom/darkpooltrading/blob/master/Screen%20Shot%202020-11-04%20at%204.55.26%20PM.png" alt="pairwise" width="450"/>

**K-Means Clustering.**
Through k-means clustering we were able to group the data points by our five key features. Initially, we created six clusters based on number of shares traded, unit price per share, close price, and time of trade, but we found time to be the most influential in our results. In order to reduce the influence of time over the clustering we ran k-means again with the additional feature of difference between unit price and close price. The largest cluster (#1) put the overall average for each feature including trade time around 12PM with 558,187 shares sold at a unit price of 15.82 and average difference of .01. It is interesting to note that in all other cluster groups the average difference is negative and average number of shares is above at least 1 million which is double our bar for what constitutes a block trade. Our fifth cluster is the only grouping with an average close and unit price of a fraction of a cent which is a significant outlier group compared to the other clusters in terms of price and low impact on the market overall. Given the consistency in our data, real-time analysis is a feasible option that could potentially better adapt given the changing market, but the data set would continue to grow forever, impacting memory use and computing time. Therefore a “forgetting” method would have to be implemented where new data is added and older data is dropped before updating centroids. 

<img src="https://github.com/carrizom/darkpooltrading/blob/master/Screen%20Shot%202020-11-04%20at%205.03.34%20PM.png" alt="kmeans vis" width="325"/>
<img src="https://github.com/carrizom/darkpooltrading/blob/master/Screen%20Shot%202020-11-04%20at%205.03.45%20PM.png" alt="kmeans clstr" width="575"/>

**Linear Regression.**
To analyze the high correlation between unit price and close price we began with simple Linear Regression modeling. Our final linear regression model was trained with 70% of the dataset randomly selected and tested on the remaining 30%. We wanted to train with as much of the data as possible, but found that the model tended to overfit if we went over 70%.  
<img src="https://github.com/carrizom/darkpooltrading/blob/master/linreg1.png" alt="Linear Regression" width="325"/>
<img src="https://github.com/carrizom/darkpooltrading/blob/master/lingreg2.png" alt="Linear Regression Plot" width="575"/>

In the end, despite the high R squared correlation value of .94 our data was not fitting well in the linear model. We attempted to normalize the data to improve the fit which reduced the R squared correlation value to .76, but still found the fit to be lacking as you can see in our plots. 

**Neural Net.**
Given the lack of a linear trend we decided to use a Neural Net. The Neural Net was given an input of 2 nodes based on 2 key features price and percent change of price before the trade occurred. The price was normalized between -1 and 1 on trades above 30 cents per unit. The first hidden layer of the net has 15 hidden units and the final layer has a single output node based on if the stock price after the block trade occurred increased (1) or decreased (0). We trained the neural net on 70% of our dataset similar to linear regression and tested it on 30%. The loss values of the gradient descent method decreased at every cycle indicating the training proceeded well and we found the training accuracy to be 66.11% while testing accuracy was at 56.80%. Overall, our testing accuracy was 6.8% over the baseline performance of 50% to decide whether a stock following a given block trade would trend positive or negative for the next seven days. This is better than baseline and is considered good based on industry standards which are around 55% for a profitable outcome, but optimally our algorithm accuracy would be around 60%. Potential ways to increase the success of our algorithm would be to consider short term and long-term stock prices and adding a layer to the neural net that considers the specific type of stock. For example, many times within our data multiple block trades would occur within the same stock in the same day or over multiple days. Our current algorithm considers each trade individual. If we were to associate block trades of the same stock or similar stocks we may be able to increase the accuracy of our predictions.  
<img src="https://github.com/carrizom/darkpooltrading/blob/master/lossrate.png" alt="Neural Net Loss Rate" width="325"/>


## Results
The results we're trying to achieve through this algorithm is creating real-time predictive algorithms that can look at data from block trades and hidden trades and can correctly predict whether it is a good idea to buy or sell a particular stock (or do nothing).

**Unsupervised Learning.**
When looking at histograms for frequency vs time we noted most of the trades happened right before market close at 5PM, illustrating it may be worth collecting closing price data 7 to 14 days before the trade occurred and 7 to 14 days after for each stock because it may take time for the block trade to impact the overall value of the stock. Then feature analysis through pairwise plots and the heat map helped us realize the low correlation through many of our features and again highlight the potential need to include more features in future data sets. Finally, the K-means analysis helped us narrow down outliers by identifying low impact clusters with little significance and see potential groupings in future analysis.  

## Discussion
The best outcome for this project would be an algorithm that correctly predicts what action to take regarding a particular stock looking at real-time trading data. It would be really cool to be able to apply the same techniques that we used on our project for more trading strategies. 

**Unsupervised Learning.**
For further analysis, it may be helpful to take out data points where block trades have a unit price under $0.30 per share because we realized in our clustering that this are outliers and by eliminating them we could get a more accurate model. Additionally, including other related features may be helpful in interpreting the data we do have given the low correlations between the features that we are currently tracking. 

## References
1. Chang, P.-C., Liu, C.-H., Lin, J.-L., Fan, C.-Y., & Ng, C. S. P. (2009). A neural network with a case based dynamic window for stock trading prediction. Expert Systems with Applications, 36(3), 6889–6898. https://doi.org/10.1016/j.eswa.2008.08.077
2. Choudhury, S., Ghosh, S., Bhattacharya, A., Fernandes, K. J., & Tiwari, M. K. (2014). A real time clustering and SVM based price-volatility prediction for optimal trading strategy. Neurocomputing, 131, 419–426. https://doi.org/10.1016/j.neucom.2013.10.002
3. KAASTRA, I., & Boyd, M. S. (1995). FORECASTING FUTURES TRADING VOLUME USING NEURAL NETWORKS: INTRODUCTION. The Journal of Futures Markets (1986-1998), 15(18), 953. https://go.openathens.net/redirector/gatech.edu?url=https://search.proquest.com/docview/225467154?accountid=11107

