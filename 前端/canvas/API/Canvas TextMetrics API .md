---
title: Canvas TextMetrics API 
date: 2022-03-23 18:11
---
canvas context measureText 方法会返回 TextMetrics 数据结构
```js
const canvas = document.createElement('canvas');
const ctx = canvas.getContext('2d');
const text = ctx.measureText('foo');
console.log(text);
```
```json
{
    actualBoundingBoxAscent: 7.139999866485596
    actualBoundingBoxDescent: 0.1399998664855957
    actualBoundingBoxLeft: -0.04999999701976776
    actualBoundingBoxRight: 15.02998161315918
    fontBoundingBoxAscent: 11
    fontBoundingBoxDescent: 3
    width: 15.449966430664062
}
```
- **fontBoundingBoxAscent**： The distance from the horizontal line indicated by the textBaseline attribute to the top of the highest bounding rectangle of all the fonts used to render the text, in CSS pixels; positive numbers indicating a distance going up from the given baseline
- **fontBoundingBoxDescent**：The distance from the horizontal line indicated by the textBaseline attribute to the bottom of the lowest bounding rectangle of all the fonts used to render the text, in CSS pixels; positive numbers indicating a distance going down from the given baseline.
- **actualBoundingBoxAscent**：The distance from the horizontal line indicated by the textBaseline attribute to the top of the bounding rectangle of the given text, in CSS pixels; positive numbers indicating a distance going up from the given baseline.
- **actualBoundingBoxDescent**：The distance from the horizontal line indicated by the textBaseline attribute to the bottom of the bounding rectangle of the given text, in CSS pixels; positive numbers indicating a distance going down from the given baseline.

To calculate the text height you can do the following:
 ```
fontHeight = fontBoundingBoxAscent + fontBoundingBoxDescent;
actualHeight = actualBoundingBoxAscent + actualBoundingBoxDescent;
```
fontHeight is the bounding box height regardless of the string being rendered. actualHeight is specific to the string being rendered.
