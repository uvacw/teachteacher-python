## Exercise 2: Working with textual data - possible solutions

```python
from glob import glob
import random
import nltk
from nltk.stem.snowball import SnowballStemmer
import spacy


infowarsfiles = glob('articles/*/Infowars/*')
infowarsarticles = []
for filename in infowarsfiles:
    with open(filename) as f:
        infowarsarticles.append(f.read())


# taking a random sample of the articles for practice purposes
articles =random.sample(infowarsarticles, 10)

```

### [Task 4](https://github.com/uvacw/teachteacher-python/blob/main/day4/exercises-1/exercise-1.md#4-perform-some-analyses): Preprocessing data

##### a. lowercasing articles

```python
articles_lower_cased = [art.lower() for art in articles]
```

##### b. tokenization

Basic solution, using the `.str` method `.split()`. Not very sophisticated, though.

```python
articles_split = [art.split() for art in articles]
```

A more sophisticated solution:

```python
from nltk.tokenize import TreebankWordTokenizer
articles_tokenized = [TreebankWordTokenizer().tokenize(art) for art in articles ]
```

##### c. removing stopwords

Define your stopwordlist:

```python
from nltk.corpus import stopwords
mystopwords = stopwords.words("english")
mystopwords.extend(["add", "more", "words"]) # manually add more stopwords to your list if needed
print(mystopwords) #let's see what's inside
```

Now, remove stopwords from the corpus:

```python
articles_without_stopwords = []
for article in articles:
    articles_no_stop = ""
    for word in article.lower().split():
        if word not in mystopwords:
            articles_no_stop = articles_no_stop + " " + word
    articles_without_stopwords.append(articles_no_stop)
```

Same solution, but with list comprehension:

```python
articles_without_stopwords = [" ".join([w for w in article.lower().split() if w not in mystopwords]) for article in articles]
```

Different--probably more sophisticated--solution, by writing a function and calling it in a list comprehension:

```python
def remove_stopwords(article, stopwordlist):
    cleantokens = []
    for word in article:
        if word.lower() not in mystopwords:
            cleantokens.append(word)
    return cleantokens

articles_without_stopwords = [remove_stopwords(art, mystopwords) for art in articles_tokenized]
```

It's good practice to frequently inspect the results of your code, to make sure you are not making mistakes, and the results make sense. For example, compare your results to some random articles from the original sample:

```python
print(articles[8][:100])
print("-----------------")
print(" ".join(articles_without_stopwords[8])[:100])
```

##### d. stemming and lemmatization

```python
stemmer = SnowballStemmer("english")

stemmed_text = []
for article in articles:
    stemmed_words = ""
    for word in article.lower().split():
        stemmed_words = stemmed_words + " " + stemmer.stem(word)
    stemmed_text.append(stemmed_words.strip())
```

Same solution, but with list comprehension:

```python
stemmed_text  = [" ".join([stemmer.stem(w) for w in article.lower().split()]) for article in articles]
```

Compare tokeninzation and lemmatization using `Spacy`:

```python
import spacy
nlp = spacy.load("en_core_web_sm")
lemmatized_articles = [[token.lemma_ for token in nlp(art)] for art in articles]
```


Again, frequently inspect your code, and for example compare the results to the original articles:


```python
print(articles[6][:100])
print("-----------------")
print(stemmed_text[6][:100])
print("-----------------")
print(" ".join(lemmatized_articles[6])[:100])
```

### [Task 5](https://github.com/uvacw/teachteacher-python/blob/main/day4/exercises-1/exercise-1.md#5-extract-information): Extract information

```Python
import nltk

tokens = [nltk.word_tokenize(sentence) for sentence in articles]
tagged = [nltk.pos_tag(sentence) for sentence in tokens]
print(tagged[0])
```

playing around with Spacy:

```python
nlp = spacy.load('en')

doc = [nlp(sentence) for sentence in articles]
for i in doc:
    for ent in i.ents:
        if ent.label_ == 'PERSON':
            print(ent.text, ent.label_ )

```          
