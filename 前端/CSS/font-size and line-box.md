---
title: font-size and line-box
date: 2022-03-21 15:14
---
字号大小就是字体的高度，例如设置字号为50px，那么它的高度如下图所示
![](./_image/2022-03-21/2022-03-21-18-55-03@2x.jpg)
>Different font-families, same font-size, give various heights
[[ lineHeight.md | 行距#lineHeight ]] 
![](./_image/2022-03-21/2022-03-21-19-41-27@2x.jpg)
半行距 = (lineHeight – fontSize) / 2。
## fontSize and content area height
```HTML
<p>
    <span class="a">Ba</span>
    <span class="b">Ba</span>
    <span class="c">Ba</span>
</p>
```
```CSS
p  { font-size: 100px }
.a { font-family: Helvetica }
.b { font-family: Gruppo    }
.c { font-family: Catamaran }
```
Using the same font-size with different font-families produce elements with various heights:
![](./_image/2022-03-21/2022-03-21-21-08-17@2x.jpg)
Even if we’re aware of that behavior, why font-size: 100px does not create elements with 100px height? I’ve measured and found these values: Helvetica: 115px, Gruppo: 97px and Catamaran: 164px
![](./_image/2022-03-21/2022-03-21-21-08-49@2x.jpg)
Although it seems a bit weird at first, it’s totally expected. The reason **lays down inside the font itself**. Here is how it works:
- `@red:a font defines its em-square (or UPM, units per em)`, a kind of container where each character will be drawn. This square uses relative units and is generally set at 1000 units. But it can also be 1024, 2048 or anything else.
- `@red:based on its relative units, metrics of the fonts are set (ascender, descender, capital height, x-height, etc.)`. Note that some values can bleed outside of the em-square.
- in the browser, `@red:relative units are scaled to fit the desired font-size`.
Let’s take the Catamaran font and open it in FontForge to get metrics:
- the em-square is 1000
- the ascender is 1100 and the descender is 540. After running some tests, it seems that browsers use the HHead Ascent/Descent values on Mac OS, and Win Ascent/Descent values on Windows (these values may differ!). We also note that Capital Height is 680 and X height is 485.
![](./_image/2022-03-21/2022-03-21-21-20-40@2x.jpg)
That means the Catamaran font uses 1100 + 540 units in a 1000 units em-square, which gives a height of 164px when setting font-size: 100px. **This computed height defines the content-area of an element **
You can think of the **content-area** as the area where the background property applies.
We can also predict that capital letters are 68px high (680 units) and lower case letters (x-height) are 49px high (485 units). As a result, 1ex = 49px and 1em = 100px, not 164px (thankfully, em is based on font-size, not computed height)
![](./_image/2022-03-21/2022-03-21-21-22-37@2x.jpg)
Before going deeper, a short note on what this it involves. When a `<p>` element is rendered on screen, it can be composed of many lines, according to its width. Each line is made up of one or many inline elements (HTML tags or anonymous inline elements for text content) and is called a **line-box**. **The height of a line-box is based on its children’s height**. The browser therefore computes the height for each inline elements, and thus the height of the line-box (from its child’s highest point to its child’s lowest point). As a result, a line-box is always tall enough to contain all its children (by default).
>  Each HTML element is actually a stack of line-boxes. If you know the height of each line-box, you know the height of an element.

```html
<p>
    Good design will be better.
    <span class="a">Ba</span>
    <span class="b">Ba</span>
    <span class="c">Ba</span>
    We get to make a consequence.
</p>
```
![](./_image/2022-03-21/2022-03-21-21-31-48@2x.jpg)
We clearly see that the second line-box is taller than the others, due to the computed content-area of its children, and more specifically, the one using the Catamaran font
 I introduced two notions: content-area and line-box. If you’ve read it well, I told that a line-box’s height is computed according to its children’s height, I didn’t say its children content-area’s height. And that makes a big difference.
Even though it may sound strange, **an inline element has two different height: the content-area height and the virtual-area height** (I invented the term virtual-area as the height is invisible to us, but you won’t find any occurrence in the spec).
- the content-area height is defined by the font metrics (as seen before)
- the virtual-area height is the line-height, and it is the height used to compute the line-box’s height
![](./_image/2022-03-21/2022-03-21-21-45-42@2x.jpg)
That being said, it breaks down the popular belief that line-height is the distance between baselines. In CSS, it is not 
![](./_image/2022-03-21/2022-03-21-21-47-09@2x.jpg)
**The computed difference of height between the virtual-area and the content-area is called the leading**. Half this leading is added on top of the content-area, the other half is added on the bottom. `@The content-area is therefore always on the middle of the virtual-area`.
Based on its computed value, the line-height (virtual-area) can be equal, taller or smaller than the content-area. In case of a smaller virtual-area, leading is negative and a line-box is visually smaller than its children.
There are also other kind of inline elements:
- replaced inline elements (`@gray:<img>, <input>, <svg>`, etc.)
- inline-block and all inline-* elements
- inline elements that participate in a specific formatting context (eg. in a flexbox element, all flex items are blocksified)
For these specific inline elements, height is computed based on their `height`, `margin` and `border` properties. **If height is auto, then line-height is used and the content-area is strictly equal to the line-height**.
![](./_image/2022-03-21/2022-03-21-21-59-01@2x.jpg)
> Inline replaced elements, inline-block/inline-* and blocksified inline elements have a content-area equal to their height, or line-height

Anyway, the problem we’re still facing is how much the line-height’s normal value is? And the answer, as for the computation of the content-area’s height, is to be found inside the font metrics.
So let’s go back to FontForge. The Catamaran’s em-square is 1000, but we’re seeing many ascender/descender values:
- generals Ascent/Descent: ascender is 770 and descender is 230. Used for character drawings. (table “OS/2”)
- metrics Ascent/Descent: ascender is 1100 and descender is 540. Used for content-area’s height. (table “hhea” and table “OS/2”)
- metric Line Gap. Used for line-height: normal, by adding this value to Ascent/Descent metrics. (table “hhea”)
In our case, the Catamaran font defines a 0 unit line gap, so line-height: normal will be equal to the content-area, which is 1640 units, or 1.64.
As a comparison, the Arial font describes an em-square of 2048 units, an ascender of 1854, a descender of 434 and a line gap of 67. It means that font-size: 100px gives a content-area of 112px (1117 units) and a line-height: normal of 115px (1150 units or 1.15). All these metrics are font-specific, and set by the font designer.
It becomes obvious that setting line-height: 1 is a bad practice. I remind you that **unitless values are font-size relative, not content-area relative**, and dealing with a virtual-area smaller than the content-area is the origin of many of our problems.
![](./_image/2022-03-22/2022-03-22-10-33-08@2x.jpg)
But not only line-height: 1. For what it’s worth, on the 1117 fonts installed on my computer (yes, I installed all fonts from Google Web Fonts), 1059 fonts, around 95%, have a computed line-height greater than 1. Their computed line-height goes from 0.618 to 3.378. You’ve read it well, 3.378!
Small details on line-box computation:
- **for inline elements, padding and border increases the background area, but not the content-area’s height (nor the line-box’s height)**. The content-area is therefore not always what you see on screen. margin-top and margin-bottom have no effect.
-** for replaced inline elements, inline-block and blocksified inline elements: padding, margin and border increases the height, so the content-area and line-box’s height**