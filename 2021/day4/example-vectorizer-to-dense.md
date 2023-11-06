
```python
import pandas as pd
texts = ["hello teachers!", "how are you today?", "what?", "hello hello everybody"]

vect = CountVectorizer()

X = vect.fit_transform(texts)
print(pd.DataFrame(X.A, columns=vect.get_feature_names()).to_string())
df = pd.DataFrame(X.toarray().transpose(), index = vect.get_feature_names())
```
