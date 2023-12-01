
## Exercise 2: NLP and feature engineering

### 1. Read in the data

Load the data...

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

Let's inspect the data, and start some pre-processing/ cleaning steps...

### 2. first analyses and pre-processing steps

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

Even more sophisticated; create your own tokenizer that first split into sentences. In this way,`TreebankWordTokenizer` works better.

```python
import regex

nltk.download("punkt")
class MyTokenizer:
    def tokenize(self, text):
        tokenizer = TreebankWordTokenizer()
        result = []
        word = r"\p{letter}"
        for sent in nltk.sent_tokenize(text):
            tokens = tokenizer.tokenize(sent)    
            tokens = [t for t in tokens
                      if regex.search(word, t)]
            result += tokens
        return result

mytokenizer = MyTokenizer()
print(mytokenizer.tokenize(articles[0]))
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


#### e. cleaning: removing punctuation, line breaks, double spaces

```python
articles[17] # print a random article to inspect.
## Typical cleaning up steps:
from string import punctuation
articles = [art.replace('\n\n', '') for art in articles] # remove line breaks
articles = ["".join([w for w in art if w not in punctuation]) for doc in articles] # remove punctuation
articles = [" ".join(art.split()) for art in articles] # remove double spaces by splitting the strings into words and joining these words again

articles[17] # print the same article to see whether the changes are in line with what you want
```

### 3. N-grams

```python
articles_bigrams = [["_".join(tup) for tup in nltk.ngrams(art.split(),2)] for art in articles] # creates bigrams
articles_bigrams[7][:5] # inspect the results...

# maybe we want both unigrams and bigrams in the feature set?

assert len(articles)==len(articles_bigrams)

articles_uniandbigrams = []
for a,b in zip([art.split() for art in articles],articles_bigrams):
    articles_uniandbigrams.append(a + b)

#and let's inspect the outcomes again.
articles_uniandbigrams[7]
len(articles_uniandbigrams[7]),len(articles_bigrams[7]),len(articles[7].split())
```

Or, if you want to inspect collocations:

```python
text = [nltk.Text(tkn for tkn in art.split()) for art in articles ]
text[7].collocations(num=10)
```

----------

### 4. Extract entities and other meaningful information

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

----


*hint: if you want to include n-grams as feature input, add the following argument to your vectorizer:*

```python
myvectorizer= CountVectorizer(analyzer=lambda x:x)
```
