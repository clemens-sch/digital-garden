#Software-Engineering 
[Learn FlexBox](https://flexboxfroggy.com/#en)

---
## # display: flex ?

when we allign `display: flex` to an element - there will be 2 axis
- main-axis (x-axis) - default: horizontal
- cross-axis (y-axis)

---
### # justify-content

align items on main-axis (x-axis)

- flex-start _(default)_: align items to left side of container
- flex-end: align items to right side of container
- center: align items to center of container
- space-between: display items with equal spacing between them
- space-around: display items with equal spacing around them
- space-evenly: display items with equal spacing between & around them

```css
display: flex;
justify-content: flex-start;
```

---
### # align-items

align items on cross-axis (y-axis)

- flex-start: align items to top of container
	- aligns items at start of cross-axis (y-axis)
- flex-end: align items to bottom of container
- center: align items at vertical center of container
- baseline: display items at baseline of container
- stretch _(default)_: stretch items to fit container

```css
display: flex;
align-items: center;
```

---
### # flex-direction

defines direction items are placed in container

- row _(default)_: placed the same as text direction
	- positioned at x-axis
- row-reverse: placed opposite to text direction
- column: placed top to bottom
- column-reverse: placed bottom to top

Important: If flex-direction is a column - `justify-content` changes vertically & `align-items` aligns horizontally

```css
display: flex;
flex-direction: column;
```

---
### # flex-flow

a shorthand for `flex-direction` & `flex-wrap` - because these two are combined often

- flex-flow: row wrap

```css
display: flex;
flex-flow: row wrap;
```

---
### # flex-wrap + align-content

when we have many items: they should not be displayed only in one line - cause of this the items are displayed very small
to fix this issue we use `flex-wrap`

- nowrap _(default)_: items are all displayed in one line
- wrap: when there is no more space available, items are displayed in next line
	- this unlocks `align-content` property
	- align-content: items can be alligned at the cross-axis (y-axis)
	- gap: items get a specific gap between them

```css
display: flex;
flex-wrap: wrap;
align-content: flex-start;
gap: 2vh;
```

---
### # flex

combines `flex-grow` & `flex-shrink` & `flex-basis`
- is the shorthand for these properties
If you set one value, it will automatically assign the values for the other properties

```css
display: flex;
flex: 1;
```

---
### # flex-grow

allows an item to grow, if there is enough space

```css
display: flex;
flex-grow: 1;
```

---
### # flex-shrink

defines how fast one item shrinks in comparison to the others

- flex-shrink: 5 - the item shrinks much faster than others
- flex-shrink: 0 - is not allowed to shrink

```css
display: flex;
flex-shrink: 5;
```

---
### # flex-basis

defines the size of the item, before the space is distributed
- if the item already has a size - but we want to override this size

```css
display: flex;
flex-basis: 300px;
```

---
### # align-self

overrides the value you set in the parent-container for an induvidual item

```css
display: flex;
align-self: center;
```

---
---