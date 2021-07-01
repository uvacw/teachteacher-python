

#### Example using a single `str` object

```python
import nltk
words = "the quick brown fox jumps over the lazy dog".split()
nltk.pos_tag(words, tagset='universal')
```

#### Example using a `list` of `str`

```python
articles = ['the quick brown fox jumps over the lazy dog', 'a second sentence']
tokens = [nltk.word_tokenize(sentence) for sentence in articles]
tagged = [nltk.pos_tag(sentence, tagset='universal') for sentence in tokens]
print(tagged[0])
```

-----

| Tag  | Meaning             | English Examples                       |
|------|---------------------|----------------------------------------|
| ADJ  | adjective           | new, good, high, special, big, local   |
| ADP  | adposition          | on, of, at, with, by, into, under      |
| ADV  | adverb              | really, already, still, early, now     |
| CONJ | conjunction         | and, or, but, if, while, although      |
| DET  | determiner, article | the, a, some, most, every, no, which   |
| NOUN | noun                | year, home, costs, time, Africa        |
| NUM  | numeral             | twenty-four, fourth, 1991, 14:24       |
| PRT  | particle            | at, on, out, over per, that, up, with  |
| PRON | pronoun             | he, their, her, its, my, I, us         |
| VERB | verb                | is, say, told, given, playing, would   |
| .    | punctuation marks   | . , ; !                                |
| X    | other               | ersatz, esprit, dunno, gr8, univeristy |

[source](https://bond-lab.github.io/Corpus-Linguistics/ntumc_tag_u.html)
