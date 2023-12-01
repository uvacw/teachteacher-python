
# Possible solution to `regex` [exercise](01tuesday-regex-exercise.md)
*Please note that alternative solutions may work just as well or even better*

## extracts URLS form a list of strings

```python
import re

for l in list_w_urls:
    m = re.findall('(?:(?:https?|ftp):\/\/)?[\w.]+\.[\w]+.', l)
    print(m)

```

`?` = matches either once or zero times  
`?:` = matches the group but does not captured it / save it.  
`\w` = matches a "word" character: a letter or digit or underbar [a-zA-Z0-9_].  


## remove everything that is not a letter or number from a list of strings

```python
for e in list_w_urls:
    print(re.sub(r'[\W_]+', ' ', e))
```

`[\W_]` = matches any non-word character (or underscore, which is weirdly enough considered a 'word character' - therefore, if we simply do `\W`, we miss the underscores)  
`+` = 1 or more times  
