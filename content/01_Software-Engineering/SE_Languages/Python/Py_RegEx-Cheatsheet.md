#Software-Engineering 

---
## # Operators

Note: every character (space, character, number) is important when it comes to a regex-creation

```python
import re
# .compile method takes regex-object (pattern)
pattern = re.compile("..")
...
```

| Operator      | Explanation                                                  |
| ------------- | ------------------------------------------------------------ |
| [0-9]         | Finds numbers                                                |
| [a-z] / [A-Z] | Finds all lower-case/upper-case characters                   |
| ...+          | Can be found one/multiple times                              |
| ...{2}        | How often a group must be repeated                           |
| ...{1, 3}     | Group min.: 1 time; max.: 3 times                            |
| ?             | Group 0 or exact 1 time                                      |
| .             | Every character is allowed                                   |
| ...\|...      | one OR other pattern to match                                |
| ^...          | finds matchin item at the beginning                          |
| $...          | finds last matching item                                     |
| [^ ]          | negation - finds everything, that does not match the pattern |
| \d            | == [0-9]                                                     |
| \D            | == [^ 0-9]                                                   |
| \s            | any whitespace character                                     |
| \S            | any non-whitespace character                                 |
| \w            | == [a-zA-Z0-9]                                               |
| \W            | == [^ a-zA-Z0-9]                                             |

---
## # Methods
### # pattern.findall()

Returns a list of matching items. For simple use-cases (returns no position).

---
### # pattern.search()

This method returns an `Match-object`, if nothing is found - it returns `None`.
A `Match-object` provides lots of methods & attributes.

---
### # pattern.finditer()

It returns `Match-objects`. Allows analysing of matching patterns (incl. positions in text).

---
### # pattern.match()

It checks if the pattern at the beginning of the text matches a certain condition. 

---
## # Examples

### # .findall()

```python
p = re.compile("5")
print(p.findall("Es ist für 5 Leute reserviert"))
# ['5']

p = re.compile("[0-9]")
print(p.findall("Hello World 123 765!"))
# ['1', '2', '3', '7', '6', '5']

p = re.compile(" [0-9] ")
print(p.findall("Hello world 1 23 765!"))
# [' 1 ']

p = re.compile("[0-9]\\.[0-9][0-9]")
print(p.findall("Hello World 3.999€"))
# ['3.99']

p = re.compile("[0-9]+")
print(p.findall("Hello World 3.99€ und 3.99$"))
#['3', '99', '3', '99']

p = re.compile(" [0-9]+\\.[0-9]{2}[€$]")
print(p.findall("Für 5 Semmeln habe ich 3.90€ bezahlt."))
print(p.findall("Für 15 Semmeln habe ich 19.50$ bezahlt."))
# [' 3.90€ ']
# [' 19.50$ ']

p = re.compile(" [0-9]?\\.[0-9]{1,3}[€$] ")
print(p.findall("Für 5 Semmeln habe ich 3.90€ bezahlt."))
print(p.findall("Für 15 Semmeln habe ich 19.50$ bezahlt."))
# [' 3.90€ ']

p = re.compile(" [0-9]+[.,][0-9]{2}[€$]? ")
print(p.findall("Für 5 Semmeln habe ich 3.90€ bezahlt."))
print(p.findall("Für 15 Semmeln habe ich 19.50$ bezahlt."))
print(p.findall("Für 150 Semmeln habe ich 190,50 bezahlt."))
# [' 3.90€ ']
# [' 19.50$ ']
# [' 190,50 ']
```

### # .search()

```python
p = re.compile("Telefonnummer")
print(p.search("Die Telefonnummer ist nicht vergeben!"))
# <re.Match object; span=(4, 17), match='Telefonnummer'>

match = p.search("Die Telefonnummer ist nicht vergeben!")
print(match.span())
print(match.start())
print(match.end())
# (4, 17)
# 4
# 17

p = re.compile("Frau|Herr")
print(p.search("Herr Müller, das ist die falsche Antwort!"))
print(re.search("Frau|Herr", "Ich darf sie darauf hinweisen, dass Frau ..."))
# <re.Match object; span=(0, 4), match='Herr'>
# <re.Match object; span=(36, 40), match='Frau'>
```
