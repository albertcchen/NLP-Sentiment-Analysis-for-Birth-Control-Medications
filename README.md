![image](https://github.com/albertcchen/dsc-phase-5-capstone/blob/main/images/birth-control-pills-1296-728-feature-thumb-732x549.jpg.webp)

# Sentiment Analysis for Birth Control from Drugs.com Reviews

---

Author: [Albert Chen](https://github.com/albertcchen)

---

## Background

The data set was provided by the UCI Machine Learning Respository and can be found [here] (https://archive.ics.uci.edu/ml/datasets/Drug+Review+Dataset+%28Drugs.com%29) and accessed on March 31st, 2023.

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

## Buisiness Problem

Customer reviews and opinions can help provide important information regarding customer preferences. From this data, an automated sentiment approach to reviewing the data regarding drugs treating particular conditions and within the different types of drugs for each condition was created. 

The plan is to train and determine which model can help predict sentiment from the reviews, and which are the most important key words in these reviews to see if there are key words that can be extracted.

---
## Data Review



---

## Data Cleaning & Feature Engineering



**Features added**

Sentiment column - Created a new column where the value is either 'Negative' or 'Positive' based on the user rating. Assigned ratings greater than 7.0 as positive and everything else as negative.

Punctuation emphasis - Counted the number of punctuations used to see if this would have an impact on our analysis

Capitalization emphasis - Counted the number of capitalizations used in the review to see if this had an impact on our analysis

**Data Cleaning**

I cleaned the text using a number of functions (clean_text) that consists of:

- Converting text to lower case
- Replacing html apostrophes with an actual apostrophe
- Expanding contractions to separate words to determine if these words are important
- Removing punctuation but ignore contractions
- Removing stopwords
- Lemmatizing words

---

## Exploratory Data Analysis

---

## Modeling

### First Simple Model

###

---

## Analysis & Results

---

## Recommendations

---

## Additional Information

Full review of my notebook can be found [here](https://github.com/albertcchen/NLP-Sentiment-Analysis-for-Birth-Control-Medications) and presentation deck can be found [here].