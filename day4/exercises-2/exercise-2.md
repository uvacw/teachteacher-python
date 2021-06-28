# Exercise 2: From text to features
----

Try to take some of the data from the [exercise of this morning](https://github.com/uvacw/teachteacher-python/blob/main/day4/exercises-1/exercise-1.md), and prepare this data for a supervised classification task. More specifically, imagine you want to train a classifier that will predict whether articles come from a fake news source (e.g., `Infowars`) or a quality news outlet (e.g., `bbc`). In other words, you want to predict `source` based on linguistic variations in the articles.

To arrive at a model that will do just that, please consider taking the following steps:

- Think about your **pre-processing steps**: what type of features will you feed your algorithm? Do you, for example, want to manually remove stopwords, or include ngrams? You can use the code you've written this morning as a starting point.

- **Vectorize the data**: Try to fit different vectorizers to the data. You can use `count` vs. `tfidf` vectorizers, with or without pruning, stopword removal, etc.

- Try out a simple supervised model. Find some inspiration [here](https://github.com/uvacw/teachteacher-python/blob/main/day4/exercises-2/possible-solution-exercise-2.md). Can you predict the `source` using linguistic variations in the articles?

- Which combination of pre-processing steps + vectorizer gives the best results?

## BONUS

- Compare that bottom-up approach with a top-down (keyword or regular-expression based) approach.
