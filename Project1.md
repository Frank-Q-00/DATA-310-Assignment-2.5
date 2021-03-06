# Project 1: House Price Prediction

   In this project, the target was to build a machine learning model based on 400 homes of which the information is gleaned from Zillow, a real estate trading website. The model integrated three factors — the square footage of the house, the number and bedrooms and the number of bathrooms — and output the predicted price of the house. We then computed the difference between the predicted price and the actual price and proceeded to analysis of the results subsequently. 

   The housing data was collected for San Antonio, TX. Among the 400 houses, the maximum value was 2.2 million dollars, and the minimum price was, surprisingly, 400 dollars, while the square footages were in a reasonable range from 700 to 4000. After examining the data frame, we decided to remove the outliers that would skew the analysis; we removed houses with price larger than 800k and lower than 10k. We also removed the houses whose square footages were marked 92<abb due to the ambiguity of this expression. After preprocessing, out resulting data frame had 385 houses left.
  
   Next, we trained the data using a neural network of three layers from Tensorflow. Each layer consisted of an array of 385 square footages, bedroom numbers or bathroom numbers. We then stacked the three layers to our x array and fit it with the 385 prices being the y array. The model was fit for 500 epochs using ‘mean square error’ as the loss function and ‘sgd’ as the optimizer. Notice that to make the data compatible with the model, we scaled the square footages by 1/1k and the actual prices by 1/100k. After fitting the model, we used it to predict the price for each house, calculated the difference between predicted and actual price, and attached the new information to the data frame. Finally, we reset the index of the data frame to show the difference in descending order: the biggest over-prediction at the top and the biggest under-prediction at the bottom.
     
![](./Project1/Project1_df.png)
  
   To give a more comprehensive analysis of the result, we employed three graphs. The first graph was a histogram that exhibited the frequency counts of the actual price and predicted price. 
   
   ![](./Project1/Project1_hist.png)
   
   In the graph, we could clearly notice that the actual prices had a much higher standard deviation and greater range. Most predicted prices were between 25k and 35k, while the actual prices ranged quite evenly between 10k and 40k. This indicated that the range of the three factors, sqfts, beds, baths were much smaller than the range of the actual prices, which implied that the three factors we had were probably not enough to predict the actual price.
   
   The second graph showed us another perspective to compare actual and predicted price. In this scatterplot, the x-axis was the actual price and the y-axis was the predicted price. 
   
   ![](./Project1/Project1_scatter.png)
   
   The red line showed where x=y. Notice that prior to making this plot, we first transformed the data with the standard scalar function. The reason was that in the previous graph, we discovered that the standard deviation between the actual prices and predicted prices were largely different. By standardizing both datasets, we could improve the accuracy of the new graph. In the scatter plot, we saw that the points appeared in clusters instead of a line. This provided another piece of evidence that the prediction wasn’t good enough. We could also notice that we had more over predictions than under predictions, but the under predictions were more scattered. This was also reflected in the data frame output where the best predictions leaned towards the under prediction ends, and the under-predicted data had higher MSE.
   
   The last graph gave us the most direct conclusion. We imported the Seaborn library and  used the heatmap to plot a pair-wise correlation matrix of the five factors: actual price, beds, baths, sqfts, and predicted price. 
   
   ![](./Project1/Project1_heat.png)
   
   We could notice that the correlation coefficients between sqft-predicted price and beds - predicted price were close to 1, which meant that sqft and beds were the primary predictors. Unfortunately, none of the factors had a high correlation coefficient with the actual price. Obviously, the heatmap informed us that our model was overfitting: it explained the much of the predicted value but little of the actual value. That said, other than the three factors we had, there must exist some other variable, for example, the geographic location, that explained the actual price. This is what we will explore in the stretch goal. 
