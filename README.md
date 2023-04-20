![image](https://github.com/albertcchen/dsc-phase-5-capstone/blob/main/images/birth-control-pills-1296-728-feature-thumb-732x549.jpg.webp)

# Sentiment Analysis for Birth Control from Drugs.com Reviews

---

Author: [Albert Chen](https://github.com/albertcchen)

---

## Background

The data set was provided by the UCI Machine Learning Respository and can be found [here](https://archive.ics.uci.edu/ml/datasets/Drug+Review+Dataset+%28Drugs.com%29) and accessed on March 31st, 2023.

This data set was the result of a study done by [Surya Kallumadi](surya@ksu.edu)  and [Felix Gräßer](felix.graesser@tu-dresden.de) in order to gain insights regarding sentiment analysis of user reviews within the pharmaceutical field. Their work can be found [here](https://dl.acm.org/doi/10.1145/3194658.3194677).

The data accessed provides patient reviews on specific drugs with related conditions and a 10 star patient rating regarding overall patient satisfaction from the website Drugs.com.

Attributes of the data are as follows:

1. drugName (categorical): name of drug 
2. condition (categorical): name of condition 
3. review (text): patient review 
4. rating (numerical): 10 star patient rating 
5. date (date): date of review entry 
6. usefulCount (numerical): number of users who found review useful

---

## Business Problem

Customer reviews and opinions can help provide important information regarding customer preferences. From this data, an automated sentiment approach to reviewing the data regarding drugs treating particular conditions and within the different types of drugs for each condition was created. 

The plan is to train and determine which model can help predict sentiment from the reviews, and which are the most important key words in these reviews to see if there are key words that can be extracted.

---

## Data Cleaning & Feature Engineering

The data was provided as a train and test set respectively. For the sake of my project, I merged both data sets into one to utilize more data. The result was a dataframe that contained approximately 215,000 instances.

From there, additional cleaning was performed in order to get rid of null values.

The resulting data set was then filtered on 'Birth Control' and selected for the top 7 occuring Birth Control drugs. I observed that there were incorrectly labeled 'Conditions' that seemed to be values for the 'usefulCount' attribute that made its way into the wrong column.

Next, I created the following features:

**Features Added**

Sentiment column - Created a new column where the value is either 'Negative' or 'Positive' based on the user rating. Assigned ratings greater than 7.0 as positive and everything else as negative.

Punctuation emphasis - Counted the number of punctuations used to see if this would have an impact on our analysis

Capitalization emphasis - Counted the number of capitalizations used in the review to see if this had an impact on our analysis

**Text Cleaning**

I cleaned the text using a number of functions (clean_text) that consists of:

- Converting text to lower case
- Replacing html apostrophes with an actual apostrophe
- Expanding contractions to separate words to determine if these words are important
- Removing punctuation but ignore contractions
- Removing stopwords
- Lemmatizing words

---

## Exploratory Data Analysis

As part of the initial exploratory data analysis, I wanted to see what the distribution of the 7 selected birth control drugs looked like according to the sentiment column we created which was based on user reviews that rated the drug on a scale from 1-10. 

![image](https://github.com/albertcchen/NLP-Sentiment-Analysis-for-Birth-Control-Medications/blob/main/images/distribution_bc_review_sentiment.png)

This first graph demonstrates how 'Levonorgestrel' has more positive reviews than negative reviews, whereas the remaining drugs have more negative reviews than positive reviews.

Next, I wanted to see if binning the time periods that these reviews were written had an impact on the drugs. I was able to observe that comparing two different time periods, 2008-2012 and 2013-2017 showed a change in sentiment for the drug 'Etonogestrel' as seen here.

![image](https://github.com/albertcchen/NLP-Sentiment-Analysis-for-Birth-Control-Medications/blob/main/images/etonogestrel_sentiment_time.png)

This would demonstrate an interesting use case as the drug has garnered more negative reviews in the more recent time period, when previously it had more positive reviews. This could provide valuable insights to any company that manufactures this particular drug, as clearly the customer sentiment has changed over time.

---

## Modeling

### First Simple Model (Logistic Regression / Count Vectorizer / No Features)

The first simple created utilized just the patient review against the sentiment that was based on their user provided rating of the drug. The text was vectorized using a CountVectorizer and then fit utilizing a logistic regression model. Cross validation was then performed on the model as a validation check and determined to be fine. I then utilized the test data to determine that this model resulted in an accuracy of 79%. 

I wanted to use accuracy as the metric of choice since it is a measure of the true positives and negatives divided by the number of all the cases, which is most useful for measuring our sentiment.

An example of the classification matrix can be seen below:

![image](https://github.com/albertcchen/NLP-Sentiment-Analysis-for-Birth-Control-Medications/blob/main/images/logistic_regression_confusion_matrix.png)

### Second Model (Logistic Regression / TFIDF Vectorizer / No Features)

The second model I created was similar to the first model, but I wanted to determine if utilizing a TFIDF Vectorizer would be more effective than using the CountVectorizer. The results of the model demonstrated accuracy improved by approximately 1%. The first iteration of this model considered if the sentiment was a three classes ('Postiive', 'Neutral', 'Negative'). However, for our business case we decided to utilize only two classes ('Positive', 'Negative') as this would be more useful as these two sentiments could be more useful in predicting patient satisfaction.

An example of the classification matrix can be seen below:

![image](https://github.com/albertcchen/NLP-Sentiment-Analysis-for-Birth-Control-Medications/blob/main/images/logistic_regression_confusion_matrix_tfidf.png)

### Third Model (Multinomial Bayes / Count Vectorizer / No Features)

The next model tested was using a Multinomial Bayes model with a CountVectorizer for the text. I wanted to be able to compare the results of using this model to that of the Logistic Regression and iterate through different models to find the best accuracy amongst the models. The results of this model yielded 82% accuracy and were the highest so far. Will continue to keep testing other models for comparison.

### Fourth Model (Logistic Regression / Count Vectorizer / All Features)

This model utilized additional features to the patient review, including 'drugName', 'condition', 'punc_empahsis', and 'capt_emphasis'. I wanted to see if adding additional features would improve the performance of the Logistic Regression model. The results of this was a model with an accuracy of 83%. This is a one percent accuracy over the Multinomial Bayes model, but utilizing more features. Therefore the model considered for final usage was still Model 3.

### Fifth Model (Logistic Regression / Count Vectorizer / Only Levonogestrel)

Based on the exploratory data analysis, Levonogestrel was the most common occurance of birth control drugs within the data set. Therefore I wanted to be able to see how using this drug by itself would perform as a model, as we could apply a similar process to other drugs as well within birth control, and additionally within different drug conditions.

Utilizing Logistic Regression as well as a CountVectorizer, the accuracy of the model was 80%, which was comparable to the other models we created.

### Sixth Model (Random Forest Classifier / TFIDF Vectorizer / All Features)

Our last model before final model evaluation was a Random Forest Classifier using a TFIDF Vectorizer as well as all the features in our data. After training the model on the training data, I passed this model through a grid search to find the best condition for the Random Forest Classifier.

### Final Model Evaluation

In addition to the Random Forest Classifier, I passed both the Logistic Regression and Multinomial Bayes models through a Grid Search in order to tune the hyperparamters.

The results were as follows:

Logit Results = {'C': 1, 'penalty': 'l2'}

NB Results = {'alpha': 0.5}

RFC results = {'criterion': 'gini', 'max_depth': 8, 'max_features': 'auto', 'n_estimators': 300}

I then applied these hyperparameters to their respective models and Multinomial Bayes performed the best with respects to accuracy compared to the other models at 82%.

---

## Analysis & Results

From the trained model I was able to create word clouds for both Negative and Positive sentiments that highlight which words appear most frequently in each sentiment.

![image](https://github.com/albertcchen/NLP-Sentiment-Analysis-for-Birth-Control-Medications/blob/main/images/word_clouds.png)

Within the repository is also the pickled file of the model for deployment as future steps.

---

## Recommendations

With this model, I provided recommendations on what can next be done with the model.

1. Sentiment Over Time - Can compare time periods to see if a particular birth control has changed sentiment, i.e more recent negative reviews or more recent positive reviews and determine what is driving this change.

2. Increased Market Awareness - Compare each type of birth control against each other and see why one might perform better than others.

3. Deployment - Deploy this process so pharmaceutical companies could use this as well and see what could be changed, i.e reformulation.

---

## Next Steps

There are a number of next steps that can be taken for this process:

1. Apply what has been learned here with birth control treatments to other conditions within the data set. 

2. Utilize BERT modeling for more advanced language processing.

3. Live integration through application implementation which would show real time data visualization, sentiment against time, and highlight key words that are associated with respective sentiment.

---

## Additional Information

Full review of my notebook can be found [here](https://github.com/albertcchen/NLP-Sentiment-Analysis-for-Birth-Control-Medications) and presentation deck can be found [here](https://github.com/albertcchen/NLP-Sentiment-Analysis-for-Birth-Control-Medications/blob/main/AC%20Capstone%20NLP%20Sentiment%20Analysis%20for%20Birth%20Control%20Medications.pdf).