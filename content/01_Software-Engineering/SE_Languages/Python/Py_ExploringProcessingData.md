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



---
### # Normalizing

.lower() function

```python
df = pd.DataFrame({"tweets" : tweets})
#df = pd.DataFrame(data=tweets, columns=["tweets"])
df["tweets"] = df["tweets"].str.lower()
#df["tweets"] = df["tweets"].apply(lambda x: x.lower())
df
```

![[Pasted image 20241107210640.png]]

unnötige zeichen entfernen

```python
df["tweets"] = df["tweets"].str.replace("[^\\w\\s+]", "", regex=True)
```