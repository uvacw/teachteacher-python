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

### 4. Vectorize the data

Imagine you want to train a classifier that will predict whether articles come from a fake news source (e.g., `Infowars`) or a quality news outlet (e.g., `bbc`). In other words, you want to predict `source` based on linguistic variations in the articles.

To arrive at a model that will do just that, you have to transform 'text' to 'features'.

- Can you vectorize the data? Try defining different vectorizers. Consider the following options:
    - `count` vs. `tfidf` vectorizers
    - with/ without pruning
    - with/ without stopword removal

### 5. Fit a classifier

- Try out a simple supervised model. Find some inspiration [here](possible-solution-exercise-day1.md). Can you predict the `source` using linguistic variations in the articles?

- Which combination of pre-processing steps + vectorizer gives the best results?

### BONUS: Inceasing efficiency + reusability
The approach under (3) gets you very far.
But for those of you who want to go the extra mile, here are some suggestions for further improvements in handling such a large dataset, consisting of thousands of files, and for deeper thinking about data handling:

- Consider writing a function to read the data. Let your function take three parameters as input, `basepath` (where is the folder with articles located?), `month` and `outlet`, and return the articles that match this criterion.
- Even better, make it a *generator* that yields the articles instead of returning a whole list.
- Consider yielding a dict (with date, outlet, and the article itself) instead of yielding only the article text.
- Think of the most memory-efficient way to get an overview of how often a given regular expression R is mentioned per outlet!
- Under which circumstances would you consider having your function for reading the data return a pandas dataframe?
