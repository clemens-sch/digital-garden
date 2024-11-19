#Software-Engineering 

---
## # PDF

In most cases, processed data is saved as a pdf-file. When extracting data from a pdf-file we use the `PyPDF2` library.

```python
import PyPDF2 as pypdf

with open("Cyber-Sicherheit.pdf", "rb") as file:
    reader = pypdf.PdfReader(file)
    print(reader.metadata) # document-info
    print(len(reader.pages)) # count of pages
# {'/Author': 'Rechnungshof', '/CreationDate': "D:20220422095428+02'00'", ...

# read a specific page of file (in this case - first page)
with open("Cyber-Sicherheit.pdf", "rb") as file:
    reader = pypdf.PdfReader(file)
    print(reader.pages[0].extract_text())
# III–623 der Beilagen zu den Stenographischen Protokollen des Nationalrates XXVII. GP ...


```

---

## # HTML

When extracting data from the WWW, we need 2 libraries: `Beautifulsoup`, `urllib`.

```python
import urllib.request as urllib2
from bs4 import BeautifulSoup
```

### # Basics

```python
# loading website
response = urllib2.urlopen("https://htlkrems.ac.at")
html_doc = response.read()

# parsing html-document & output of first 1000 characters
soup = BeautifulSoup(response, "html.parse")
strhtml = soup.prettify()
print(strhtml[:1000])

# navigate in soup-structure
print(soup.title)
# <title>HTL Krems</title>
print(soup.title.string)
# HTL Krems
```

### # urllib

```python
response = urllib2.urlopen("https://...")
html_doc = response.read()
```

### # BeautifulSoup

```python
# declaration
soup = BeautifulSoup(html_doc, "html.parse")
strhtm = soup.prettify()

# navigation
print(soup.title.string)

# find specific tags/elements
print(soup.find_all("a")) # here, it just finds all "a" tags
for e in elements:        # here, we only get the text
    print(e.text)
```
