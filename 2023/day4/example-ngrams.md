## N-grams

```python
import nltk
from gensim import corpora
from gensim import models

documents = ["In the train from Connecticut to New York",
             "He is a spokesman for New York City's Health Department",
             "New York has been one of the states hit hardest by Coronavirus"]

documents_bigrams = [["_".join(tup) for tup in nltk.ngrams(doc.split(),2)] for doc in documents]

len(documents) == len(documents_bigrams)
# maybe we want both unigrams and bigrams in the feature set?
documents_uniandbigrams = []
for a,b in zip([doc.split() for doc in documents],documents_bigrams):
    documents_uniandbigrams.append(a + b)

print(documents_uniandbigrams)
```

if you want to use this as input for a `sklearn` classifier, you can do the following:

```python
myvectorizer = CountVectorizer(analyzer=lambda x:x)
```

And if you want to see what's happening, convert to a dense format (please only do this with a small toy sample, never on a large dataset):

```python
X = myvectorizer.fit_transform(documents_uniandbigrams)
df = pd.DataFrame(X.toarray().transpose(), index = myvectorizer.get_feature_names())
df
documents_uniandbigrams
​
myvectorizer = CountVectorizer(analyzer=lambda x:x)
X = myvectorizer.fit_transform(documents_uniandbigrams)
df = pd.DataFrame(X.toarray().transpose(), index = myvectorizer.get_feature_names())
```

### Collocations with `NLTK`

```python
import nltk
documents = ["He travelled by train from New York to Connecticut and back to New York",
             "He is a spokesman for New York City's Health Department",
             "New York has been one of the states hit hardest by Coronavirus"]

text = [nltk.Text(tkn for tkn in doc.split()) for doc in documents ] # this inspects frequencies WITHIN documents
text[0].collocations(num=10)
```

### Collocations with `Gensim`

```python
from nltk.tokenize import TreebankWordTokenizer
import pandas as pd
import regex
from sklearn.feature_extraction.text import CountVectorizer
from gensim.models import KeyedVectors, Phrases
from gensim.models.phrases import Phraser
from glob import glob

infowarsfiles = glob('articles/*/Infowars/*')
documents = []
for filename in infowarsfiles:
    with open(filename) as f:
        documents.append(f.read())

mytokenizer = TreebankWordTokenizer()
tokenized_texts = [mytokenizer.tokenize(t) for t in documents]

phrases_model = Phrases(tokenized_texts, min_count=10, scoring="npmi", threshold=.5)
score_dict = phrases_model.export_phrases()
scores = pd.DataFrame(score_dict.items(),
columns=["phrase", "score"])
scores.sort_values("score",ascending=False).head()
```

Using `Gensim`'s collocations in `sklearn`'s vectorizer

```python
from gensim.models.phrases import Phraser
import numpy as np

phraser = Phraser(phrases_model)
tokens_phrases = [phraser[doc] for doc in tokens]
cv = CountVectorizer(tokenizer=lambda x: x, lowercase=False) # initiate a count or tfidf vectorizer
```

Inspecting the resulting dtm

```python
from gensim.models.phrases import Phraser
import numpy as np

phraser = Phraser(phrases_model)
tokens_phrases = [phraser[doc] for doc in tokens]
cv = CountVectorizer(tokenizer=lambda x: x, lowercase=False) # initiate a count or tfidf vectorizer



def termstats(dfm, vectorizer):
    """Helper function to calculate term and document frequency per term"""
    # Frequencies are the column sums of the DFM
    frequencies = dfm.sum(axis=0).tolist()[0]
    # Document frequencies are the binned count
    # of the column indices of DFM entries
    docfreqs = np.bincount(dfm.indices)
    freq_df=pd.DataFrame(dict(frequency=frequencies,docfreq=docfreqs), index=vectorizer.get_feature_names())
    return freq_df.sort_values("frequency", ascending=False)

dtm = cv.fit_transform(tokens_phrases)
termstats(dtm, cv).filter(like="hussein", axis=0)
```
