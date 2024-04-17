#Software-Engineering 

---
## # What are CSS Selectors?

They are powerful tools that allow you to target and style specific elements on a web page. 
We will have a look at some important Selectors along with examples.

---
### # Basic Selectors

#### # Universal Selector (`*`)

selects all elements on the page

```css
* {
	margin: 0;
	padding: 0;
}
```
---
#### # Element Selector (`htmlelement`)

selects all instances of a specific HTML element

```css
p {
	color: blue
}
```
---
#### # Class Selector (`.classname`)

selects elements with a specific class attribute

```css
.classname {
	background-color: yellow;
}
```
---
#### # ID Selector (`#idname`)

selects a single element with a specific ID attribute

```css
#idname {
	font-size: 24px
}
```
---
#### # Descendant Selector (`element1 element2`)

selects all elements with name element2 that are descendant of elements1

```css
.element1 .element2 {
	font-style: italic;
}
```
---
#### # Child Selector (`element1 > element2`)

selects all element2 elements where the parent is an element1 element

```css
ul > li {
	list-style-type: square;
}
```
---
### # Pseudo Selectors

#### # :hover Selector

gets activated, when you hover over element

```css
a:hover {
	color: blue;
}
```
---
#### # ::before & ::after Selector

`::before` creates a virtual content before the actual content of the element
`::after` creates a virtual content after the actual content of the element

```css
p::before {
	content: "Before";
	font-weight: bold;
}

p::after {
	content: "After";
	color: green;
}
```
---
---
