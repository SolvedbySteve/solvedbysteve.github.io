---
title: "Credit Predictor Bot Project"
date: 2021-06-14
tags: [data engineering, data science, machine learning, deep learning, aws lambda, aws lex, feature engineering]
header:
  image: "/images/credit-bot/real-hero.jpeg"
excerpt: "Data Engineering, Data Science, Machine Learning, Deep learning, AWS Lambda, AWS Lex, Feature Engineering"
mathjax: "true"
---

# Credit Predictor Bot

## The Problem 

For this project I really wanted to embrace the spirit of Fintech, so I decided to add a spin to a popular [Kaggle](https://www.kaggle.com/uciml/default-of-credit-card-clients-dataset) dataset. My idea is that I can pitch our credit consulting bot to major credit card providers. My bot allows customers to find out how likely they are to default on their payment using various machine learning methods, it even gives them a percentage! Potential customers interact with an Amazon Lex bot that is powered by An Amazon Lambda function.

I believe that this type of analysis is extremely valuable to financial institutions as it allows the business to accuraately detect potential default which impacts the bottom line; The bot also gives the customers of the business a tool to correct reckless credit behavior prior to defaulting on their loan.

<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/chatbot2.png" alt="chatbot">




## Approach to the Solution


I decided to test several methods of machine learning prior to deploying our Lex Bot. We used the Logistic Regression, Naive Random Oversampling, SMOTE, SMOTEEN, and Clustered Centroid machine learning methods to solve this problem. To determine the best model we looked at accuracy score and the imbalanced classification reports. I also used deep learning to cover all bases.

<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/init_corr.PNG" alt="initial corr">

## Initial Results

As you will see below, my initial results weren't very good. After I downloaded the dataset from Kaggle, we inspected the data-set by using the .corr method in Python to see which columns were most correlated to our target. I then created the X and y training variables. Our initial results were very low. When I used the data in the Simple Logistic Regression model, the results (seen below) were not favorable. Our initial Balanced Accuracy Score was 59%; I also saw that the Recall Score was at 21% for predicting default (1s) and our F1 score was at 33%. Based on these results, I knew that I needed to make changes immediately.

<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/init_corr.PNG" alt="initial corr">
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/init_scores.PNG" alt="initial scores">



## Feature Engineering

<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/engineer.png" alt="engineer">



While the data-set provided us with 30,000 rows of data, one of the limitations is that it doesn't provide much context. Ideally, I'd like to see columns include information such as, "Income," "Debt to Income Ratio," "Previous Default" etc. Since I didn't have the data I felt like I needed, my challenge was to create new features using the data that was provided. This made our project challenging yet rewarding.

The most correlated columns were the PAY0-PAY6 columns. These features represented if the customer made or missed payments within the last 6 months. I decided that I should make a feature that represents the total amount of payments that the customer missed in the last six months. From there I created a feature that measures the percentage of payments that the customer has missed (Ex: 2 payments missed is 2/6 or 33.33%).

Finally, I created features that measure the customer's total bill for 6 months as well as the total payment amount for the same time period.

### Creating the new features
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/newfeatures.PNG" alt="newfeatures">

### Showing the new correlation
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/newfeatcorr.PNG" alt="newfeatcorr">

### New scores
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/newscores.PNG" alt="newscores">

When I ran the .corr function after added the features, I discovered that I had created features that were more correlated to our taget than the pre-existing features. I also saw that every score increased, therefore, based on the new results I felt comfortable moving forward with our testing. Below you will see every method that I used to predict credit card default. You will see the creation of the pickle file(these are used to load the data into Lambda), accuracy score, balanced accuracy report and an example of the bot that I made in Lex for each method.

## Logistic Regression Model
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/logregmodel.jpg" alt="logregpic">

Below are the scores for the Logistic Regression model. You will notice that the balanced accuracy score is 3% better than the initial results. It is also clear that the prediction of default (1) increased by 4%.


### Logistic regression scores
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/logregscores.JPG" alt="logregscores">

## Naive Random Oversampling

<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/naive.JPG" alt="naivepic">

Below are the Naive Random Oversampling results. You will see that while the balanced accuracy score increased by 8% over our initial results, the prediction and recall scores were significantly lower than the previous model. I continued to look for the perfect machine learning match.

### NAIVE Random Oversampling scores
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/naivescores.JPG" alt="naivescores">

## Clustered Centroid
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/cluster.JPG" alt="clusterpic">


Below are the Clustered Centroid results. This method barely outperformed the original test in regards to the balanced accuracy score. It performed worse in every other measurable category. I decided against using this method.

### Clustered Centroid scores
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/clusterscore.JPG" alt="clusterscores">

## SMOTE
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/smotepic.jpg" alt="smotepic">

These are the Smote results. The balanced accuracy score is 10% higher than the original results, however, the ability to predict default is ability to predict default is only 49%; This metric is actually worse than the original data.

### SMOTE scores
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/smotescores.JPG" alt="smotescores">

## SMOTEENN
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/smoteennpic.png" alt="smoteennpic">

The Smoteenn results were very similar to the Smote example. This method has the best balanced accuracy score, however it was one of the worst at predicting default.

### SMOTEEN scores
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/smoteennscores.JPG" alt="smoteennscores">

## Deep Learning
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/deeplearningpic.jpg" alt="deeplearningpic">

Because the standard machine learning methods rendered results that were inconsistent, I turned our attention to deep learning. I created one single layer model and one multi-layered model and tested them against each other to see which one returned the most favorable results.

### Single layer DEEP LEARNING
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/deeplearningsingle.JPG" alt="deeplearningsingle">

### Multiple layer DEEP LEARNING
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/deeplearningmulti.JPG" alt="deeplearningmulti">

### Evaluation the models
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/plot1.JPG" alt="plot1">
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/plot2.JPG" alt="plot2">
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/plot3.JPG" alt="plot3">



## Final Verdict
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/deep_dive.jpg" alt="deepdive">

The deep learning methods were the best methods to solve this problem. Though there were small differences between the single and multiple hidden layer methods, the single layer method performed better. The single hidden layer has a loss metric of 17.3% and an accuracy of 78% for training while testing data shows 17.9% loss and 77% accuracy. Even though the multi hidden layered method had a similar accuracy score for training and testing data, the testing data outperformed the training data which implies that this method is slightly overfit.

If I was using this to solve a similar problem for a potential client, I would use this method because, in theory I was able to increase the accuracy by almost 20% when compared to the original results.

## Chat Bots
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/bot2.png" alt="bot2">

For each machine learning model I created a chat bot using Amazon Lex and Amazon Lambda. I saved each model to a pickle file. This means that I can send these files to anyone in the world and the model is ready to be deployed; There is no need to train the model or manipulate the data. This solution is truly "plug and play."

### Machine learning AWS Lambda function
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/smoteennlambda.JPG" alt="bot2">

### Maching learning bot video
place holder for bot video ![alt text](image.jpg)

### Deep Learning lambda function
<img src="{{ site.url }}{{ site.baseurl }}/images/credit-bot/deeplearnlambda.JPG" alt="bot2">

### Deep learning bot video
place holder for image ![alt text](image.jpg)






