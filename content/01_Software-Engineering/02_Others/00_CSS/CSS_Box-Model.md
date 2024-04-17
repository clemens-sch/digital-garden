#Software-Engineering 

---
## # A Box?

At first, there is to say that everything you have is a box.
Does not matter if it is a div, span, ....
Every single element is a box. 

---
## # Box Model

If you have a look the the Browser-Dev-Tools you can inspect the box model.
In this example you can see, that:
- the whole box is the box model
- the inside (blue) is the actual content of the box

The inside can be specified using the `height` & `width` properties

![[Pasted image 20240203113116.png]]

---
## # Box Properties
### # margin

expands the size of the box
- outside the element - out of the border
- if element1 has margin-bottom: 60px
	& element2 has margin-top: 60px - _margin collapses_ (there will only be 60px space - not 120!) 

in this example, there will be a space on every side by 20px outside the element

```css
.box {
	margin: 60px;
	margin: top right bottom left;
	margin: 10px 20px 30px 40px;
}
```
---
### # border

expands the size of the box
- inside the element - expands the border

in this example, the border gets thicker to 20px inside the element

```css
.box {
	border: 30px solid #9B51E0;
}
```
---
### # padding

expands the size of the box
- inside the element - inside the border

in this example, everything will get pushed away by 20px inside the element

```css
.box {
	padding: 10px;
	padding: top right bottom left;
	padding: 10px 20px 30px 40px;
}
```
---
### # height & width

sets the height & width of the content in a box (element)
- inside the element - inside the border - inside the padding

in this example, the content gets a 100px * 100px surface

```css
.box {
	height: 100px;
	width: 100px;
}
```
---

now, the box model will look like this

![[Pasted image 20240203114314.png]]

---
## # box-sizing

### # border-box

the Browser calculates the total width & height of an element
- the width & height include padding & borders

### # content-box (default)

the width & height of an element exclude padding and borders
- is applied to the element's content area only

```css
.box {
	box-sizing: border-box;
	box-sizing: content-box; default
}
```

---