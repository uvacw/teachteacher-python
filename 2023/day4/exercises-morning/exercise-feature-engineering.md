# Exercise 2: NLP and feature engineering
----

In this exercise, you can use one of yesterday's datasets (IMDB or the newspaper data). Today, we will use this data for analysis and feature extraction using NLP. These are important components of feature engineering: moving from textual data to a feature set that can be used in a classification model.

### 1. Read in the data

You can use the code you've written yesterday as a starting point. Again, try your code on a small sample of the data, and scale up later--once your confident that your code works as intended.

### 2. first analyses and pre-processing steps

- Perform some first analyses on the data using string methods and regular expressions.
Techniques you can try out include:

a.  lowercasing  
b.  tokenization  
c.  stopword removal  
d.  stemming and/or lemmatizing)  
e.  cleaning: removing punctuation, line breaks, double spaces  


### 3. N-grams

- Think about what type of n-grams you want to add to your feature set. Extract and inspect n-grams and/or collocations, and add them to your feature set if you think this is relevant.

### 4. Extract entities and other meaningful information

Try to extract meaningful information from your texts. Depending on your interests and the nature of the data, you could:

- use regular expressions to distinguish relevant from irrelevant texts, or to extract substrings
- use NLP techniques such as Named Entity Recognition to extract entities that occur.

### 5. Train a supervised classifier

Go back to your code belonging to yesterday's assignment. Perform the same classification task, but this time carefully consider which feature set you want to use. Reflect on the options listed above, and extract features that you think are relevant to include. Carefully consider **pre-processing steps**: what type of features will you feed your algorithm? Do you, for example, want to manually remove stopwords, or include ngrams? Use these features as input for your classifier, and investigate the effects hereof on performance of the classifier. Not that the purpose is not to build the perfect classifier, but to inspect the effects of different feature engineering decisions on the outcomes of your classification algorithm.


## BONUS

- Compare that bottom-up approach with a top-down (keyword or regular-expression based) approach.
