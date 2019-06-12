# Business-Analysis-Project
R Project-Online News Popularity

## 1. Objective
 we are trying to predict whether a news would be popular by looking its features. 
 Our hypothesis is it may be hard for us to predict the actually number of shares for a 
 news article, but we should be able to make prediction on whether it would be popular,
 i.e. whether number of shares would go over the threshold. We assume entertainment topics 
 to be more popular for they catch audiences’ eyes and is easy to talk about—people likes 
 gossip. Key words should also play important rule in popularity.
 
## 2. Data Description:
The data used is ‘Online News Popularity Data Set’ from UCI Machine Learning Repository(http://archive.ics.uci.edu/ml/datasets/Online+News+Popularity).
It is a data set with 61 attributes and 39,797 instances (news articles on Mashable). It contains two non-predictive information, which are url of news articles and days between publication and data acquisition. The useful information are the remaining 59 columns, covers characteristic about words used, link attached, article channel (which is classified into 6 categories), shares of referenced articles in Mashable, publish day in week and some result from text mining (including polarity, sentiment and subjectivity of title and content).

## 3. Methodologies:
In this part, we first draw pictures to have a glance of the data. After that, we fit a simple linear regression model to the data and find it not useful. Giving the fact that it is hard for us to predict the actual number of shares, we turn the question in to classification problem. We applied Logistic Regression with Lasso Regression, SVM, Random Forest and KNN and find that Random Forest performs best.
After reading csv file as data frame, we dropt the first and second column, say, non-predictive url and time delta. To explore the data, we first look at the target variable which reflect the popularity—‘shares’.

From the table and histogram above we can find that since mean number is even larger than 3rd quantile of the number of shares, most number of shares are below 2800 (3rd quantile) and very few big news got shared almost million times; the data is positively skewed.
Then we integrate the dummy variables represent different channel and weekdays, e.g. “data_channel_is_lifestyle” and “data_channel_is_bus” into variables called “data channel” and “weekday”. The boxplot below excludes those instance with shares larger than 90% quantile for having a readable picture.

From boxplots we can find that there is no significant difference within weekdays, its weekend that makes a bit different. However, different channels do have different level of popularity. Maybe we can say the weekdays and channels are not the domain parameter for the popularity.

The correlation matrix tells us that the correlation between parameters and the target variable is not strong, indicating further operation for an admirable result.
Using 1.5 IQR as threshold for outlier and scaling the non-dummy variables, we perform a simple linear regression function (shares to all parameters) to see whether we can predict the number of shares. Screenshot of results are attached in the Appendix. We get an adjusted R-squared at only 0.1108. After selecting significant variables from former regression model, we end up at a worse adjusted R-squared: 0.109. 
PCA is applied. The first 25 principle components contain about 90% information. Fit regression model again, an even lower adjusted R-squared: 0.0647. We cannot predict the actual number of shares. We should transform it into a classification question.

Choosing the median as threshold, about 49.34% news are popular; a quite balance data. In the following model training part, if not specified, the train, test set is split by 7:3.

### 3.1 KNN:
We first try the most intuitive KNN model, after a loop from K=1 to K=10, and accuracy as criteria, we find 9 is the best value for K, the result accuracy would be 60.39% and the AUC is 0.472, which is far from a satisfactory result.

|F1 Score |Precision|Recall|
|--------|:-------:|:-------:|
|0.565|0.605|0.530|

### 3.2  Logistic Regression:
If linear regression cannot apply to this data set, what about the logistic regression? We applied the logistic regression, get an accuracy at 62.80%, which looks acceptable. After selection of the significant data, the accuracy increase to 64.76%. The AUC is 0.697.

|F1 Score |Precision|Recall|
|--------|:-------:|:-------:|
|0.632	|0.640	|0.624|

### 3.3 LASSO:
We then try penalty based variable selection method, LASSO to see whether further improvement could be achieved on Logistic regression. Set family equals to “binomial” would produce a logistic regression formula. After all we have accuracy equals to 65.02%, and AUC equals to 0.702, a step forward from models above.

|F1 Score |Precision|Recall|
|--------|:-------:|:-------:|
|0.637	|0.642	|0.631|

### 3.4 Supported Vector Machine:
For SVM is also a binary classification tool, we also give it a try, using 50% train, test split due to capability of machine, using default settings we get accuracy at 64.28% and AUC equals to 0.698.

|F1 Score |Precision|Recall|
|--------|:-------:|:-------:|
|0.635	|0.636	|0.633|

### 3.5 Gradient Boosting:
No model above outperforms other models very much, we then tried the Gradient Boosting Model, we have drawn the following conclusions: the maximum of accuracy, 0.6731966 appears when shrinkage equals 1 and maximum interaction depth equals 3. The AUC is 0.732, much better than former models.

|F1 Score |Precision|Recall|
|--------|:-------:|:-------:|
|0.661|	0.660|	0.661|

### 3.6 Random Forest:
Finally, we tried Random Forest. Again, we use half- half split due to machine capability. As shown in figure 15, the Error of the model decreases significantly as the number of trees increases, but when number of trees is larger than 80, changes of error slow down to almost zero . Figure 16 shows the confusion matrix and the accuracy of the model. According to the confusion matrix, the precision is 0.655, the recall is 0.655, the F-measure is 0.655, the accuracy equals to 66.17%. Figure 17 shows the ROC curve, the area under curve is 1 which is extremely good.

|F1 Score |Precision|Recall|
|--------|:-------:|:-------:|
|0.655|	0.655|	0.655|

 ## 4. Results and Conclusion:
We compare the ROC of all 6 models tried above and find GBM model is the most powerful model, it outperforms Random Forest, 3 regression model does not have much difference in AUC, and KNN totally fails the competition. The most important 2 variables in the best model is the average share and maximum share of best performed key words, key words do largely affect the popularity. In top 10 variables (see appendix figure 5), 4 is topics and it is the entertainment channel that has largest explain power. The popularity of a news article do rely on topics and popular keywords. What’s more, number of share of referenced article also plays important role, which may reflect 1. Articles’ topic is correlated to the popular ones. 2. Author of the article has good writing ability to refer to popular articles. In conclusion, Authors can get idea to write super popular article—follow heat events, write entertainment topics, and publish in weekend. 

 
