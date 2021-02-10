# Exercise 1
## 2.5.2021
Questions from 1st Laurence Moroney's video and estimating house price. 

## Question 1
Description: In Laurence Moroney's video, What is ML, he compared traditional programming with machine learning and argues that the main difference between the two is a reorientation of the rules, data and answers. According to Moroney, what is the difference between traditional programming and machine learning?

Answer: In traditional programming, when encountering a problem, we already know the rules to solve the problem. Then the program inputs the data we collect into the rules and get the answer. In machine learning, on the other hand, the rules are unknown. We input the answers along with the data and asks our program to figure out rules to solv the problem. 

## Question 2
Modify the predict function to produce the output for the value 7.

Answer: first trial, 22.002361; second trial, 22.000013. The answers are NOT the same. The reason is because although the two datasets appear to have a linear relationship Y = 3X + 1, the program doesn't see it this way. It treats our input as part of a larger data set of which the relationship might not be linear. Therefore, in each approximation, the program makes a distinct prediction. Nonetheless, the difference in predictions would be minimal if the iteration number is large, as the mean of the predictions will converge to a certain point. 

## Question 3
Estimate which home is a better deal. 

Answer: the newptcomfort is the best deal according to my model, while moon is the worst deal. I trained the model with the area of the house as the x-vector and the price of the house as the y-vector. Then, I predicted what the house price should be according to the model and then subtracted the actual price. If the output is positive, the house has lower price than predicted, which means it's a good deal. Otherwise, if the output is negative, it's a bad deal. The outputs are: church:-43k, hudgins: 53k, mathews: -45k, mobjack: 54k, moon: -80k, newptcomfort: 56k
