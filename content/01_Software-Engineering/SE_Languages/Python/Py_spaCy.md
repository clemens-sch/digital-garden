#Software-Engineering 

[spacy](https://spacy.io/api/token)

---
## # spaCy ? 

Is an open-source library, that can be used for NLP-tasks.
It's a really powerful library compared to the nltk-library (deprecated).

---
## # Init

```python
import spacy
nlp = spacy.load("de_core_news_sm")
doc = nlp("Your text here.")
```

When you take a closer look at the doc-object, you can see that for every word the type is from `Token` (`spacy.tokens.token.Token`)
- Note: for a token-element there are lots of attributes & methods

