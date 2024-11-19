#Definitions 

[[AI_NLP]]

---
## # Working with Text

When working with NLP, there are several types of texts. 
Including unstructured texts, sentences or documents.
- When processing data - make sure to follow the NLP-Pipeline

---
## # NLP-Pipeline

The common NLP-pipeline consists of 3 stages.
Each stage transforms text in some way & produces another result that the next stage needs.

![[Pasted image 20241024220026.png]]

---
### # Text Processing

First step of NLP-pipeline.
Takes raw input text, cleans it, normalizes it and converts it into a form that suits for feature extraction.

There are 5 steps, you have to follow:

![[Pasted image 20241024221817.png]]

---
### # 7 Steps - Text Processing

1. **Cleaning**
	1. removing irrelevant items - HTML-tags, ...
	2. Regex can be used
2. **Normalization**
	1. converting all words to lowercase
	2. removing punctuation & extra spaces
3. **Tokenization**
	1. split text into words ("tokens")
	3. libraries used: nltk/spacy
4. **Stop words removal**
	1. most common words - a, an, the, etc, ... ("stop words") are removed
5. **Parts of Speech Tagging**
	1. determines each word's grammatical category
	2. to find connections in phrase
	3.  _Example_: “The quick brown fox jumps over the lazy dog.”
		1. “The” is tagged as determiner (DT)
		2. “quick” is tagged as adjective (JJ)
		3. “brown” is tagged as adjective (JJ)
		4. “fox” is tagged as noun (NN)
		5. ...
6. **Named Entity Recognition**
	1. recognizing named entities in data
	2. *Example*: "John Snow, CEO of John Snow Lab."
		1. **Person:** John Snow (CEO)
		2. **Organization:** John Snow Lab
7. **Stemming and Lemmatization**
	1. **Stemming**
		1. reduces word to root form - cutting off suffixes & prefixes without considering the words meaning
		2. approach: strip endings like "-ing", "-ed", or "-es"
		3. *Example*: "run", "running", "runner" = "run"/"runn"
	2. **Lemmatization**
		1. reduces word to base form - considers the word's meaning and grammatical context
		2. approach: uses dictionary to find correct root form
		3. *Example*: "is, are, was, were" = "be"

---
## # Feature Extraction

Is a process of transforming raw text data into a structured format that ML-algorithms can work with. 
- It identifies important pieces of information (features) from the text 

---
## # Modeling



