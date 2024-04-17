#Software-Engineering 

---
## # CSS Positioning ?

### # position: static (default) 

Default position of element is `static`. 
- `static` means, that elements are positioned as the HTML structure is
- with a `static` position you don't have access to top, right, bottom, left properties 

---
### # position: relative

gets "removed" from the document-flow (no longer works as `static` element)
allows to `relative` change the position of element from where it would be normally positioned
- top, right, bottom, left 

---
### # position: absolute

completely "removes" the element from the document-flow - everything else renders like the absolute-element didn't even exist
The parent element has to implement some other position than `static` - then the child element (which is `absolute`) will use it as parent
- top, right, bottom, left 

---
### # position: fixed

similar to `absolute` positioning
Are always fixed positioned based on the entire HTML-elements
They stay at the sameplace when you scroll

---
### # position: sticky

combination of `absolute` & `fixed` position
when you scroll down, the element will be on top of the side

---
## # Additional CSS Properties

some usefull properties when working with posititons may be:

`z-index: -1` - moves the item in the background (index: -1)
`opacity: 0.3` - can make the item invisible
`overflow...: hidden` - there will be no scrollbar (the item will be cut of)

```css
.item1 {
	position: absolute;
	z-index: -1;
	opacity: 0.3;
	overflow-x: hidden;
	overflow-y: hidden;
}
```

---
---