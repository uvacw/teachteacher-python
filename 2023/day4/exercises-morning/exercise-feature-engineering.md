# Exercise 1: Working with textual data

### 0. Get the data.

- Download `articles.tar.gz` from
https://dx.doi.org/10.7910/DVN/ULHLCB

If you experience difficulties downloading this (rather large) dataset, you can also download just a part of the data [here](https://surfdrive.surf.nl/files/index.php/s/bfNFkuUVoVtiyuk)

- Unpack it. On Linux and MacOS, you can do this with `tar -xzf mydata.tar.gz` on the command line. On Windows, you may need an additional tool such as `7zip` for that (note that technically speaking, there is a `tar` archive within a `gz` archive, so unpacking may take *two* steps depending on your tool).


### 1. Inspect the structure of the dataset.
What information do the following elements give you?

- folder (directory) names
- folder structure/hierarchy
- file names
- file contents

### 2. Discuss strategies for working with this dataset!

- Which questions could you answer?
- How could you deal with it, given the size and the structure?
- How much memory<sup>1</sup> (RAM) does your computer have? How large is the complete dataset? What does that mean?
- Make a sketch (e.g., with pen&paper), how you could handle your workflow and your data to answer your question.

<sup>1</sup> *memory* (RAM), not *storage* (harddisk)!

### 3. Read some (or all?) data

Here is some example code that you can modify. Assuming that he folder `articles` is in the same folder as the notebook you are currently working on, you could, for instance, do the following to read a *part* of your dataset.

```python
from glob import glob
infowarsfiles = glob('articles/*/Infowars/*')
infowarsarticles = []
for filename in infowarsfiles:
    with open(filename) as f:
	    infowarsarticles.append(f.read())

```

- Can you explain what the `glob` function does?
- What does `infowarsfiles` contain, and what does `infowarsarticles` contain? First make an educated guess based on the code snippet, then check it! Do *not* print the whole thing, but use `len`, `type` en slicing `[:10]` to get the info ou need.

- Tip: take a random sample of the articles for practice purposes (if your code works, you can scale up!)

```
# taking a random sample of the articles for practice purposes
articles =random.sample(infowarsarticles, 10)
```

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
