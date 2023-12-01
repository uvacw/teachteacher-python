## Exercise 1 Working with textual data - possible solutions

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
