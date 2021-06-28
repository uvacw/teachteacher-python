
### Exercise 2: From text to features:
### Trying out different preprocessing steps

Load the data...

```python
infowarsfiles = glob('articles/*/Infowars/*')
documents = []
for filename in infowarsfiles:
    with open(filename) as f:
        documents.append(f.read())
```

Let's inspect the data, and start some pre-processing/ cleaning steps

```python
## From text to features.
documents[17] # print a random article to inspect.
## Typical cleaning up steps:
from string import punctuation
documents = [doc.replace('\n\n', '') for doc in documents] # remove line breaks
documents = ["".join([w for w in doc if w not in punctuation]) for doc in documents] # remove punctuation
documents = [doc.lower() for doc in documents] # covert to lower case
documents = [" ".join(doc.split()) for doc in documents] # remove double spaces by splitting the strings into words and joining these words again

documents[17] # print the same article to see whether the changes are in line with what you want
```

Removing stopwords:

```python
mystopwords = set(stopwords.words('english')) # use default NLTK stopword list; alternatively:
# mystopwords = set(open('mystopwordfile.txt').readlines())  #read stopword list from a textfile with one stopword per line
documents = [" ".join([w for w in doc.split() if w not in mystopwords]) for doc in documents]
documents[7]
```

Using N-grams as features:

```python
documents_bigrams = [["_".join(tup) for tup in nltk.ngrams(doc.split(),2)] for doc in documents] # creates bigrams
documents_bigrams[7][:5] # inspect the results...

# maybe we want both unigrams and bigrams in the feature set?

assert len(documents)==len(documents_bigrams)

documents_uniandbigrams = []
for a,b in zip([doc.split() for doc in documents],documents_bigrams):
    documents_uniandbigrams.append(a + b)

#and let's inspect the outcomes again.
documents_uniandbigrams[7]
len(documents_uniandbigrams[7]),len(documents_bigrams[7]),len(documents[7].split())
```

Or, if you want to inspect collocations:

```python
text = [nltk.Text(tkn for tkn in doc.split()) for doc in documents ]
text[7].collocations(num=10)
```

----------

### Vectorize the data

```python
from glob import glob
import random

def read_data(listofoutlets):
    texts = []
    labels = []
    for label in listofoutlets:
        for file in glob(f'articles/*/{label}/*'):
            with open(file) as f:
                texts.append(f.read())
                labels.append(label)
    return texts, labels

X, y = read_data(['Infowars', 'BBC']) #choose your own newsoutlets

```


```python
#split the dataset in a train and test sample
from sklearn.model_selection import train_test_split
X_train,X_test,y_train,y_test=train_test_split(X, y, test_size=0.2)    
```

Define some vectorizers.
You can try out different variations:
- `count` versus `tfidf`
- with/ without a stopword list
- with / without pruning


```python
from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer

myvectorizer= CountVectorizer(stop_words=mystopwords) # you can further modify this yourself.

#Fit the vectorizer, and transform.
X_features_train = myvectorizer.fit_transform(X_train)
X_features_test = myvectorizer.transform(X_test)

```
### Build a simple classifier

Now, lets build a simple classifier and predict outlet based on textual features:

```python
from sklearn.naive_bayes import MultinomialNB
from sklearn.metrics import accuracy_score
from sklearn.metrics import classification_report

model = MultinomialNB()
model.fit(X_features_train, y_train)
y_pred = model.predict(X_features_test)

print(f"Accuracy : {accuracy_score(y_test, y_pred)}")
print(classification_report(y_test, y_pred))

```

Can you improve this classifier when using different vectorizers?


*hint: if you want to include n-grams as feature input, add the following argument to your vectorizer:*

```python
myvectorizer= CountVectorizer(analyzer=lambda x:x)
```
