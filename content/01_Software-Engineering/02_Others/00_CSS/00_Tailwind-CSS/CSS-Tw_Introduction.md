
---
## # What is Tailwind-CSS ?



---
### # Personalization




---
### # Side notes

Tailwind only creates classes - it needs

creating own Colors-padding:

```css
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    screens: {
      sm: "480px",
      md: "768px",
      lg: "976px",
      xl: "1440px",
    },
    spacing: {
      1: "8px",
      2: "12px",
      3: "16px",
      4: "32px",
      5: "50px",
      187: "200px"
    },

    extend: {
      colors: {
        ownColor: {
          100: "#5F9EA0",
          200: "#FF5733",
        },
      },
    },
  },
  plugins: [],
};
```


in the index.css we can specifiy a specific look for a specific element

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  html {
    @apply bg-slate-900;
    @apply text-ownColor-100;
  }
}
```


### # typography

...

### # Spacing

`padding`

padding; padding-left; padding-right; padding-top; padding-bottom;
padding-x axis (left + right); padding-y axis (top + bottom)

```css
p-1
pl-1
pr-1
pt-1
pb-1
---
px-1
py-1
```


`margin`


`width`
```css
w-full
```


`space between`


### # flex

`flex`

main-content : `flex-grow`


### # grid

column (x-axis)
row (y-axis)

`grid`
`grid-cols-2` - horizontal allignment (^v)
`grid-rows-2` - vertical allignment (<-->)

`gap`
`col-span-2` - one element takes 2 times the width
`row-span-2` - one element takes 2 times the height 

`col-start-1` `col-end-1`

`grid-flow-row-dense` 


### # Layout

`container`
- `mx-auto` - center container
- `px-1` - sets some padding on left, right

#### # position

static
relative
absolute

#### # overflow

`overflow-hidden`


#### # borders

`border` - adds a 1px border all around


#### # Effects & Shadows

`shadow-md`
`shadow-cyan-500/50`
`opacity-5`
`blur-sm`
`brightness-125`

`transition`

`scale-150` - make an image smaller/bigger


### # core concepts

#### # breakpoint prefix

breakpoint prefix - like @media-queries
- for responsive sites

...

---
### # darkmode


