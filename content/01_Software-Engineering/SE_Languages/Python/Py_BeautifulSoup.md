#Software-Engineering 

[Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)

---
## # BeautifulSoup ?

A powerful library, that allows scraping data from websites using python.

---
## # Intro

```python
import urllib.request as urllib2
from bs4 import BeautifulSoup
```

```python
# loading website
response = urllib2.urlopen('https://www.htlkrems.ac.at')
html_doc = response.read()

# represents full HTML-document
soup = BeautifulSoup(html_doc, 'html.parser')

# formated HTML-document structure
strhtm = soup.prettify()
# output
print(strhtm[:1000])
```

Output

![[Pasted image 20240928101354.png]]

---
## # Element data

```python
# get element-data 
print(soup.title)
print(soup.title.string)
# <title>HTL Krems</title>
# HTL Krems
```

---
## # .find_all()

```python
# text from every anchor-tag on website
for tag in soup.find_all("a"):
    print(tag.text)
```

```python
# count all hyperlinks of the website
links = soup.find_all("a")
print(len(links))
```
