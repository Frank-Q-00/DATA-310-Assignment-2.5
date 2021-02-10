# Exercise 2
Questions from 2nd Laurence Moroney's video and computer vision

## Question 1
**Why do we split the dataset to training and testing?**

Answer: Training and testing sets for machine learning models are like study guides and exams for students. The study guide provides students with information of a new topic, while the exams check how well they learn this topic. Same things happen for machine learning models. The training set allows the model to find patterns between the data and labels and the testing set assesses the accuracy of the pattern on unknown data. Without the training set, we cannot build the model. Conversely, we cannot determine if our model is effective in general. 


## Question 2
**Purposes of the three layers and the functions**

Answer: The first layer compresses our input of a 28 by 28 size image into one single neuron. The second layer contains 128 neurons that process the input, and the relu function followed after it filters the negative values. This is because negative values could cancel out positive values, causing a skew in the output. The third layer contains 10 neurons which indicate 10 possible classes that our input image might belong to. The softmax function calculates the probabilities of the input in a certain class; then it fixes the largest value to one, telling us that which class the model believes that our image should be in. 


## Question 3
**How do the optimizer and loss functions operate to produce estimates**

Answer: When we pass in the data and labels into our model, the model doesn't know the relationship between them. Therefore, it has to make a guess. The loss function will take the initial guess, evaluate whether the guess is a good one, and pass it to the optimizer which makes a slightly better guess. The new guess is passed into the loss function and the loop continues until the model determines the relationship between the data and the label. 


## Question 4
**Draw datasets and answer the following questions**



