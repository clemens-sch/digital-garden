#Software-Engineering 

[[AI_ExploringProcessingData]]

---
## # Intro

When it comes to working with text, there also exists unstructured data. 
If we want to train a model, we have to clean up the training-data to improve the quality & training-duration of the model.
When speaking about unstructured data, we have to translate the text for our model to structured data.

---
## # 1. Text-Processing

### # Cleaning

Use Regex to clean the data.
This step removes things like <html/>-tags, characters like "/}"...

---
### # Normalizing

Use .lower() function to transfer the text in smaller-case. 
Remove punctuations from the text.

```python
df = pd.DataFrame({"tweets" : tweets})
#df = pd.DataFrame(data=tweets, columns=["tweets"])
df["tweets"] = df["tweets"].str.lower()
#df["tweets"] = df["tweets"].apply(lambda x: x.lower())
df
```

![[Pasted image 20241107210640.png]]

```python
df["tweets"] = df["tweets"].str.replace("[^\\w\\s+]", "", regex=True)
```

---
### # Tokenize

Seperate the text to "Tokens" (one sentence - seperated word by word)

- NLTK
- [[Py_spaCy]]
	- [spaCy-YT Tutorial](https://www.youtube.com/watch?v=_lR3RjvYvF4)

---
### # Stopwords

Stopwords are words, that often appear in our language.
Examples would be "a, the, ..."
Image searching in your search engine "How to develop chatbot using Python" - Here, the search engine will remove the stopwords "how, to"

---
### # Stemming

---
### # Lemmatization

---
### # POS

"Part Of Speech-Tagging"

POS using NLTK:

```python
import nltk

from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from nltk import pos_tag
nltk.download('stopwords')
nltk.download('punkt')
nltk.download('averaged_perceptron_tagger_eng')
# for a list of categories
nltk.download('tagsets_json')

text = "I love NLP and I will learn NLP in 2 month"
stop_words = set(stopwords.words('english'))
words = [token for token in word_tokenize(text) if token not in stop_words]
print(nltk.pos_tag(words)) 

# [('I', 'PRP'), ('love', 'VBP'), ('NLP', 'NNP'), ('I', 'PRP'), ('learn', 'VBP'), ('NLP', 'RB'), ('2', 'CD'), ('month', 'NN')]
```

In this example you can see, that "I" is a Personal-pronoum. "love" is a Verb. ...

List word-categories

```python
# note: the import above is needed!
nltk.help.upenn_tagset()
```


POS using spacy:

```python
import spacy

nlp = spacy.load("en_core_web_sm")
text = "I love NLP and I will learn NLP in 2 month"
doc = nlp(text)

words=[token for token in doc if token.is_stop == False]
for token in words:
 print(token.text, token.pos_) 

# love VERB
# NLP PROPN
# learn VERB
# NLP PROPN
# 2 NUM
# month NOUN
```

Note: "I" is also identified as stopword. So it gets removed