# A Practical Introduction to Machine Learning in Python
Anne Kroon and Damian Trilling

This is just one solution. Maybe you came up with an even better one yourself!

### Reading the files in:

```python
from striprtf.striprtf import rtf_to_text

# read the files in
filenames = ["news_corona_" + str(i) + ".RTF" for i in range(1, 4) ]
rtf_string  = [ open("exercises-afternoon/corona_news/" + f).read() for f in filenames ]

# convert the files from rtf to string format
text = [ rtf_to_text(i) for i in rtf_string ]

# replace line breaks and split articles

splitted_text = [ i.replace("\n", " ").split("End of Document  ") for i in text ]

```

### A function that parses the documents.

```python
import re

def parse_nexis_uni(news_string):
    ''' parses strings (nexis news articles), so that the title, date and full text are extracted. '''

    parsed_results = []
    for line in news_string:

        # newspaper title
        matchObj1=re.match(" +([a-zA-Z\s]+?) \d+",line)
        if matchObj1:
            newspaper = matchObj1.group(1)
        else:
            newspaper = "NaN"

        # date
        matchObj2 = re.match(r".*(\d{1,2}) ([jJ]anuari|[fF]ebruari|[mM]aart|[aA]pril|[mM]ei|[jJ]uni|[jJ]uli|[aA]ugustus|[sS]eptember|[Oo]ktober|[nN]ovember|[dD]ecember) (\d{4}).*", line)
        if matchObj2:
            day = matchObj2.group(1)
            month = matchObj2.group(2)
            year = matchObj2.group(3)
            date = (day, month, year )
        else:
            date = "NaN"

        # full text
        matchObj3=re.match(".*Body(.*) Classification",line)
        if matchObj3:
            text = matchObj3.group(1).strip()
        else:
            text = "NaN"

        parsed_results.append( {'newspaper': newspaper,
                       'date' : date,
                       'text': text }  )

    return parsed_results

```

#### calling the function

```python
results = []
for document in splitted_text:
    results.extend(parse_nexis_uni(document))
```
