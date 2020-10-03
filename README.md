# Machine Learning For Institutional Advanced Order Recognition & Market Reaction Opportunities
![Infographic](https://github.com/carrizom/darkpooltrading/blob/master/Infographic.png?raw=true)
## Introduction
Institutional financial firms use specialized orders and deep industry liquidity to fill asset trades at preferable rates and high volume. In this project, we will focus on dark-pool block trades, market-maker (MM) preferred trades, and hide-not-slide orders. All of these trading mechanisms are allowed and encouraged to an extent by central exchanges and regulators through lax regulation and skimpy enforcement of clearing laws due to technical loopholes in (international) trade execution venues. For instance, firms can delay reporting of billion-dollar trades for up to 48 hours by routing orders from New York to a British trade desk and back to the states for clearing. These practices, combined with other particular orders that bypass book liquidity (hide-not-slide, pegged, etc.) enable firms to conceal their market intentions and transact at away-from-market prices not available to the general public at the time of reporting. We intend to measure these sorts of trades and use machine learning to implement 0+ HFT and options-swing-trading algorithms to profit by mimicking "smart" investments and other specialized market opportunities based on hidden institutional trades.

## Methods
For dark pool trading, we intend on creating a dataset using historical data from Street Smart Edge for transactions were more than 500,000 shares were sold. The variables we're interested in analyzing are price discrepancy (price that the trade was closed at vs current price of the stock) and size of the order. We want to cluster our data using this variables to determine whether we should buy a stock, sell it or do nothing. We also plan on using neural networks to determine the correct action for a trade. 

For the 0+ HFT and options-swing-trading algorithms we intend on creating our own dataset utilizing historic data from Bloomberg. We will be focusing on hidden trades and book feed. We're still in the process of determining what the best ML approach would be for this particular dataset. At the moment we're considering regression, clustering, and neural networks.

## Results
The results we're trying to achieve through this algorithm is creating real-time predictive algorithms that can look at data from block trades and hidden trades and can correctly predict whether it is a good idea to buy or sell a particular stock (or do nothing). 

## Discussion
The best outcome for this project would be an algorithm that correctly predicts what action to take regarding a particular stock looking at real-time trading data. It would be really cool to be able to apply the same techniques that we used on our project for more trading strategies. 

## References
1. Chang, P.-C., Liu, C.-H., Lin, J.-L., Fan, C.-Y., & Ng, C. S. P. (2009). A neural network with a case based dynamic window for stock trading prediction. Expert Systems with Applications, 36(3), 6889–6898. https://doi.org/10.1016/j.eswa.2008.08.077
2. Choudhury, S., Ghosh, S., Bhattacharya, A., Fernandes, K. J., & Tiwari, M. K. (2014). A real time clustering and SVM based price-volatility prediction for optimal trading strategy. Neurocomputing, 131, 419–426. https://doi.org/10.1016/j.neucom.2013.10.002
3. KAASTRA, I., & Boyd, M. S. (1995). FORECASTING FUTURES TRADING VOLUME USING NEURAL NETWORKS: INTRODUCTION. The Journal of Futures Markets (1986-1998), 15(18), 953. https://go.openathens.net/redirector/gatech.edu?url=https://search.proquest.com/docview/225467154?accountid=11107

