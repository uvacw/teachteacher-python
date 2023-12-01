# A Practical Introduction to Machine Learning in Python
Anne Kroon and Damian Trilling

## Day 2 (Tuesday Afternoon)

## Exercise: Parsing unstructured text files

When working with text data, often we have to deal with unstructured files. Before we can start with our analysis, we have to transform such files to more structured forms of data.

An example of such forms of unstructured data is the output of Nexis Uni, a large news database often used by social scientists.
We will practise with some files downloaded from Nexis Uni.

Download and unpack a set of .RTF files [here](corona_news.tar.gz).
Windows users may need an additional program to unpack it, such as 7zip.

Specific tasks

1. Write some code to read the data in.
3. Try to extract the newspaper title using regular expressions.
4. Do the same for the publication dates.
5. Finally, extract the full body of the text.
6. Think about a way to store the data


Hints:

In order to read .RTF files with python, we need to convert rtf files to strings, before we can start parsing and processing.
This library can help: https://pypi.org/project/striprtf/

```bash
pip install striprtf
```

Afterwards, we can start converting our files:

```python
from striprtf.striprtf import rtf_to_text

rtf_string = open("exercises-afternoon/corona_news/news_corona_1.RTF").read()
text = rtf_to_text(rtf_string)

```

This will return a string object. In order to split up the string by article, we can look at the structure of the data.
As you might notice, all news articles went with 'End of Document  '. We can use this information to split the string.

```python
splitted_text = text.replace("\n", " ").split("End of Document  ")
```
