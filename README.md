# Documentation of the core of Mozilla's PDF.js library
## Operations
### Preliminary mark
The list of operations contains all operations as defined in the PDF Specification that are used in PDF.js, aside with the operations that are only defined by PDF.js. The first group of operations show the operator, operands and description as defined in the PDF Specification. In the second group I tried to describe the operations as good as possible.

### Overview
#### General graphics state
* [`setLineWidth`](#2-setlinewidth-w) (`w`)
* [`setLineCap`](#3-setlinecap-j) (`J`)
* [`setLineJoin`](#4-setlinejoin-j) (`j`)
* [`setMiterLimit`](#5-setmiterlimit-m) (`M`)
* [`setDash`](#6-setdash-d) (`d`)
* [`setRenderingIntent`](#7-setrenderingintent-ri) (`ri`)
* [`setFlatness`](#8-setflatness-i) (`i`)
* [`setGState`](#9-setgstate-gs) (`gs`)

#### Special graphics state
* [`save`](#10-save-q) (`q`)
* [`restore`](#11-restore-q) (`Q`)
* [`transform`](#12-transform-cm) (`cm`)

#### Path construction
* [`moveTo`](#13-moveto-m) (`m`)
* [`lineTo`](#14-lineto-l) (`l`)
* [`curveTo`](#15-curveto-c) (`c`)
* [`curveTo2`](#16-curveto2-v) (`v`)
* [`curveTo3`](#17-curveto3-y) (`y`)
* [`closePath`](#18-closepath-h) (`h`)
* [`rectangle`](#19-rectangle-re) (`re`)

#### Path painting
* [`stroke`](#20-stroke-s) (`S`)
* [`closeStroke`](#21-closestroke-s) (`s`)
* [`fill`](#22-fill-f--f) (`f` / `F`)
* [`eoFill`](#23-eofill-f) (`f*`)
* [`fillStroke`](#24-fillstroke-b) (`B`)
* [`eoFillStroke`](#25-eofill-stroke-b) (`B*`)
* [`closeFillStroke`](#26-closefillstroke-b) (`b`)
* [`closeEOFillStroke`](#27-closeeofillstroke-b) (`b*`)
* [`endPath`](#28-endpath-n) (`n`)

#### Clipping paths
* [`clip`](#29-clip-w) (`W`)
* [`eoClip`](#30-eoclip-w) (`W*`)

#### Text objects
* [`beginnText`](#31-beginntext-bt) (`BT`)
* [`endText`](#32-endtext-et) (`ET`)

#### Text state
* [`setCharSpacing`](#33-setcharspacing-tc) (`Tc`)
* [`setWordSpacing`](#34-setwordspacing-tw) (`Tw`)
* [`setHScale`](#35-sethscale-tz) (`Tz`)
* [`setLeading`](#36-setleading-tl) (`TL`)
* [`setFont`](#37-setfont-tf) (`Tf`)
* [`setTextRenderingIntent`](#38-settextrenderingintent-tr) (`Tr`)
* [`setTextRise`](#39-settextrise-ts) (`Ts`)

#### Text positioning
* [`moveText`](#40-movetext-td) (`Td`)
* [`setLeadingMoveText`](#41-setleadingmovetext-td) (`TD`)
* [`setTextMatrix`](#42-settextmatrix-tm) (`Tm`)
* [`nextLine`](#43-nextline-t) (`T*`)

#### Text showing
* [`showText`](#44-showtext-tj) (`Tj`)
* [`showSpacedText`](#45-showspacedtext-tj) (`TJ`)
* [`nextLineShowText`](#46-nextlineshowtext-) (`'`)
* [`nextLineSetSpacingShowText`](#47-nextlinesetspacingshowtext-) (`"`)

#### Type 3 fonts
* [`setCharWidth`](#48-setcharwidth-d0) (`d0`)
* [`setCharWidthAndBounds`](#49-setcharwidthandbounds-d1) (`d1`)

#### Color
* [`setStrokeColorSpace`](#50-setstrokecolorspace-cs) (`CS`)
* [`setFillColorSpace`](#51-setfillcolorspace-cs) (`cs`)
* [`setStrokeColor`](#52-setstrokecolor-sc) (`SC`)
* [`setStrokeColorN`](#53-setstrokecolorn-scn) (`SCN`)
* [`setFillColor`](#54-setfillcolor-sc) (`sc`)
* [`setFillColorN`](#55-setfillcolorn-scn) (`scn`)
* [`setStrokeGray`](#56-setstrokegray-g) (`G`)
* [`setFillGray`](#57-setfillgray-g) (`g`)
* [`setStrokeRGBColor`](#58-setstrokergbcolor-rg) (`RG`)
* [`setFillRGBColor`](#59-setfillrgbcolor-rg) (`rg`)
* [`setStrokeCMYKColor`](#60-setstrokecmykcolor-k) (`K`)
* [`setFillCMYKColor`](#61-setfillcmykcolor-k) (`k`)

#### Shading patterns
* [`shadingFill`](#62-shadingfill-sh) (`sh`)

#### Inline images
* [`beginInlineImage`](#63-begininlineimage-bi) (`BI`)
* [`beginImageData`](#64-beginimagedata-id) (`ID`)
* [`endInlineImage`](#65-endinlineimage-ei) (`EI`)

#### XObjects
* [`paintXObject`](#66-paintxobject-do) (`Do`)

#### Marked content
* [`markPoint`](#67-markpoint-mp) (`MP`)
* [`markPointProps`](#68-markpointprops-dp) (`DP`)
* [`beginMarkedContent`](#69-beginmarkedcontent-bmc) (`BMC`)
* [`beginMarkedContentProps`](#70-beginmarkedcontentprops-bdc) (`BDC`)
* [`endMarkedContent`](#71-endmarkedcontent-emc) (`EMC`)

#### Compatibility
* [`beginCompat`](#72-begincompat-bx) (`BX`)
* [`endCompat`](#73-endcompat-ex) (`EX`)

#### PDF.js only
* [`dependency`](#1-dependency)
* [`paintFormXObjectBegin`](#74-paintformxobjectbegin)
* [`paintFormXObjectEnd`](#75-paintformxobjectend)
* [`beginGroup`](#76-begingroup)
* [`endGroup`](#77-endgroup)
* [`beginAnnotations`](#78-beginannotations)
* [`endAnnotations`](#79-endannotations)
* [`beginAnnotation`](#80-beginannotation)
* [`endAnnotation`](#81-endannotation)
* [`paintJpegXObject`](#82-paintjpegxobject)
* [`paintImageMaskXObject`](#83-paintimagemaskxobject)
* [`paintImageMaskXObjectGroup`](#84-paintimagemaskxobjectgroup)
* [`paintImageXObject`](#85-paintimagexobject)
* [`paintInlineImageXObject`](#86-paintinlineimagexobject)
* [`paintInlineImageXObjectGroup`](#87-paintinlineimagexobjectgroup)
* [`paintImageXObjectRepeat`](#88-paintimagexobjectrepeat)
* [`paintImageMaskXObjectRepeat`](#89-paintimagemaskxobjectrepeat)
* [`paintSolidColorImageMask`](#90-paintsolidcolorimagemask)
* [`constructPath`](#91-constructpath)

### 1: dependency
* `dependencies`: *`array`* - list of dependency object ids

### 2: setLineWidth (`w`)
#### Operands
* `lineWidth`: *`number`*

#### PDF Specification
Set the line width in the graphics state.

#### Source
* [PDF Specification / Table 57 – Graphics State Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793795)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2823](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2823)

### 3: setLineCap (`J`)
#### Operands
* `lineCap`: *`number`* - either `0` (butt), `1` (round), or `2` (square)

#### PDF Specification
Set the line cap style in the graphics state.

#### Source
* [PDF Specification / Table 57 – Graphics State Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793795)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2824](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2824)
* https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/lineCap

### 4: setLineJoin (`j`)
#### Operands
* `lineJoin`: *`number`* - either `0` (miter), `1` (round), or `2` (bevel)

#### PDF Specification
Set the line join style in the graphics state.

#### Source
* [PDF Specification / Table 57 – Graphics State Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793795)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2825](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2825)
* https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/lineJoin

### 5: setMiterLimit (`M`)
#### Operands
* `miterLimit`: *`number`*

#### PDF Specification
Set the miter limit in the graphics state.

#### Source
* [PDF Specification / Table 57 – Graphics State Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793795)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2826](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2826)
* https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/miterLimit

### 6: setDash (`d`)
#### Operands
* `dashArray`: *`array`* - An array of numbers which specify distances to alternately draw a line and a gap (in coordinate space units). If the number of elements in the array is odd, the elements of the array get copied and concatenated;

  see also: https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/setLineDash
* `dashPhase`: *`number`* - A float specifying the amount of the offset;

  see also: https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/lineDashOffset

#### PDF Specification
Set the line dash pattern in the graphics state.

#### Source
* [PDF Specification / Table 57 – Graphics State Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793795)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2827](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2827)

### 7: setRenderingIntent (`ri`)
#### Operands
* `intent`: ?

#### PDF Specification
Set the colour rendering intent in the graphics state.

#### Source
* [PDF Specification / Table 57 – Graphics State Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793795)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2828](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2828)

### 8: setFlatness (`i`)
#### Operands
* `flatness`: *`number`*

#### PDF Specification
Set the flatness tolerance in the graphics state. `flatness` is a number in the range `0` to `100`; a value of `0` shall specify the output device’s default flatness tolerance.

#### Source
* [PDF Specification / Table 57 – Graphics State Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793795)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2829](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2829)

### 9: setGState (`gs`)
#### Operands
* `states`: *`array`* - An array of key-value pairs where the first element of the pair is the key and the second element is the value.
  Keys may be any of the following:
  * `LW` ([setLineWidth](#2-setlinewidth-w))
  * `LC` ([setLineCap](#3-setlinecap-j))
  * `LJ` ([setLineJoin](#4-setlinejoin-j))
  * `ML` ([setMiterLimit](#5-setmiterlimit-m))
  * `D` ([setDash](#6-setdash-d))
  * `RI` ([setRenderingIntent](#7-setrenderingintent-ri))
  * `FL` ([setFlatness](#8-setflatness-i))
  * `Font` ([setFont](#37-setfont-tf))
  * `CA` (stroke alpha)
  * `ca` (fill alpha)
  * `BM` (global composite operation)
  * `SMask`.

#### PDF Specification
Set the specified parameters in the graphics state. `dictName` shall be the name of a graphics state parameter dictionary in the **ExtGState** subdictionary of the current resource dictionary.

#### Source
* [PDF Specification / Table 57 – Graphics State Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793795)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2830](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2830)

### 10: save (`q`)
#### Operands
*none*

Saves the following properties:
* font
  * font size
  * font size scale
  * font weight (only svg)
  * font matrix
* text
  * text matrix
  * text matrix scale
  * text horizontal scale
  * text rendering mode
  * text rise
* spacing
  * character spacing
  * word spacing
* color
  * fill color
  * stroke color
* alpha
  * alpha is shape (only canvas)
  * fill alpha
  * stroke alpha
* line
  * line width
  * line join (only svg)
  * line cap (only svg)
* leading ?
* pattern fill ?
* current point (x, y) (in user coordinates)
* start of text line (x, y) (in text coordinates)
* active s-mask
* resume s-mask context
* dash array (only svg)
* dash phase (only svg)
* dependencies (only svg)
* active clip url (only svg)
* clip group (only svg)
* mask id (only svg)

#### PDF Specification
Save the current graphics state on the graphics state stack.

#### Source
* [PDF Specification / Table 57 – Graphics State Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793795)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2831](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2831)

### 11: restore (`Q`)
#### Operands
*none*

#### PDF Specification
Restore the graphics state by removing the [most recently saved state](#10-save-q) from the stack and making it the current state.

#### Source
* [PDF Specification / Table 57 – Graphics State Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793795)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2832](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2832)

### 12: transform (`cm`)
#### Operands
* `a`: *`number`* - horizontal scaling
* `b`: *`number`* - horizontal skewing
* `c`: *`number`* - vertical skewing
* `d`: *`number`* - vertical scaling
* `e`: *`number`* - horizontal moving
* `f`: *`number`* - vertical moving

#### PDF Specification
Modify the current transformation matrix (CTM) by concatenating the specified matrix. Although the operands specify a matrix, they shall be written as six separate numbers, not as an array.

#### Source
* [PDF Specification / Table 57 – Graphics State Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793795)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2833](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2833)

### 13: moveTo (`m`)
#### Operands
* `x`: *`number`*
* `y`: *`number`*

#### PDF Specification
Begin a new subpath by moving the current point to coordinates (`x`, `y`), omitting any connecting line segment. If the previous path construction operator in the current path was also **`m`**, the new **`m`** overrides it; no vestige of the previous **`m`** operation remains in the path.

#### Source
* [PDF Specification / Table 59 – Path Construction Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987642)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2836](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2836)

### 14: lineTo (`l`)
#### Operands
* `x`: *`number`*
* `y`: *`number`*

#### PDF Specification
Append a straight line segment from the current point to the point (`x`, `y`). The new current point shall be (`x`, `y`). 

#### Source
* [PDF Specification / Table 59 – Path Construction Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987642)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2837](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2837)

### 15: curveTo (`c`)
#### Operands
* `x1`: *`number`*
* `y1`: *`number`*
* `x2`: *`number`*
* `y2`: *`number`*
* `x3`: *`number`*
* `y3`: *`number`*

#### PDF Specification
Append a cubic Bézier curve to the current path. The curve shall extend from the current point to the point (`x3`, `y3`), using (`x1`, `y1`) and (`x2`, `y2`) as the Bézier control points. The new current point shall be (`x3`, `y3`). 

#### Source
* [PDF Specification / Table 59 – Path Construction Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987642)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2838](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2838)

### 16: curveTo2 (`v`)
#### Operands
* `x2`: *`number`*
* `y2`: *`number`*
* `x3`: *`number`*
* `y3`: *`number`*

#### PDF Specification
Append a cubic Bézier curve to the current path. The curve shall extend from the current point to the point (`x3`, `y3`), using the current point and (`x2`, `y2`) as the Bézier control points. The new current point shall be (`x3`, `y3`). 

#### Source
* [PDF Specification / Table 59 – Path Construction Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987642)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2839](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2839)

### 17: curveTo3 (`y`)
#### Operands
* `x1`: *`number`*
* `y1`: *`number`*
* `x3`: *`number`*
* `y3`: *`number`*

#### PDF Specification
Append a cubic Bézier curve to the current path. The curve shall extend from the current point to the point (`x3`, `y3`), using (`x1`, `y1`) and (`x3`, `y3`) as the Bézier control points. The new current point shall be (`x3`, `y3`). 

#### Source
* [PDF Specification / Table 59 – Path Construction Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987642)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2840](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2840)

### 18: closePath (`h`)
#### Operands
*none*

#### PDF Specification
Close the current subpath by appending a straight line segment from the current point to the starting point of the  
subpath. If the current subpath is already closed, **`h`** shall do nothing. 

This operator terminates the current subpath. Appending another segment to the current path shall begin a new subpath, even if the new segment begins at the endpoint reached by the **`h`** operation. 

#### Source
* [PDF Specification / Table 59 – Path Construction Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987642)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2841](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2841)

### 19: rectangle (`re`)
#### Operands
* `x`: *`number`*
* `y`: *`number`*
* `width`: *`number`*
* `height`: *`number`*

#### PDF Specification
Append a rectangle to the current path as a complete subpath, with lower-left corner (`x`, `y`) and dimensions `width` and `height` in user space. The operation

```
x y width height re
```
is equivalent to
```
x y m
(x + width) y l
(x + width) (y + height) l
x (y + height) l
h
```

#### Source
* [PDF Specification / Table 59 – Path Construction Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987642)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2842](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2842)

### 20: stroke (`S`)
#### Operands
*none*

#### PDF Specification
Stroke the path.

#### Source
* [PDF Specification / Table 60 – Path-Painting Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987646)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2843](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2843)

### 21: closeStroke (`s`)
#### Operands
*none*

#### PDF Specification
[Close](#18-closepath-h) and [stroke](#20-stroke-s) the path. This operator shall have the same effect as the sequence `h S`. 

#### Source
* [PDF Specification / Table 60 – Path-Painting Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987646)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2844](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2844)

### 22: fill (`f` / `F`)
#### Operands
*none*

#### PDF Specification
Fill the path, using the [nonzero winding number rule](http://en.wikipedia.org/wiki/Nonzero-rule) to determine the region to fill. Any subpaths that are open shall be implicitly closed before being filled.

#### Source
* [PDF Specification / Table 60 – Path-Painting Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987646)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2845-L2846](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2845-L2846)

### 23: eoFill (`f*`)
#### Operands
*none*

#### PDF Specification
Fill the path, using the [even-odd rule](http://en.wikipedia.org/wiki/Even%E2%80%93odd_rule) to determine the region to fill.

#### Source
* [PDF Specification / Table 60 – Path-Painting Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987646)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2847](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2847)

### 24: fillStroke (`B`)
#### Operands
*none*

#### PDF Specification
[Fill](#22-fill-f--f) and then [stroke](#20-stroke-s) the path, using the [nonzero winding number rule](http://en.wikipedia.org/wiki/Nonzero-rule) to determine the region to fill. This operator  shall produce the same result as constructing two identical path objects, painting the first with `f` and the second with `S`.

#### Source
* [PDF Specification / Table 60 – Path-Painting Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987646)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2848](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2848)

### 25: eoFillStroke (`B*`)
#### Operands
*none*

#### PDF Specification
[Fill](#23-eofill-f) and then [stroke](#20-stroke-s) the path, using the [even-odd rule](http://en.wikipedia.org/wiki/Even%E2%80%93odd_rule) to determine the region to fill. This operator shall produce the same result as `B`, except that the path is  filled  as  if  with `f*` instead of `f`.

#### Source
* [PDF Specification / Table 60 – Path-Painting Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987646)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2849](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2849)

### 26: closeFillStroke (`b`)
#### Operands
*none*

#### PDF Specification
[Close](#18-closepath-h), [fill](#22-fill-f--f), and then [stroke](#20-stroke-s) the path, using the [nonzero winding number rule](http://en.wikipedia.org/wiki/Nonzero-rule) to determine the region to fill. This operator shall have the same effect as the sequence `h B`.

#### Source
* [PDF Specification / Table 60 – Path-Painting Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987646)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2850](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2850)

### 27: closeEOFillStroke (`b*`)
#### Operands
*none*

#### PDF Specification
[Close](#18-closepath-h), [fill](#23-eofill-f), and then [stroke](#20-stroke-s) the path, using the [even-odd rule](http://en.wikipedia.org/wiki/Even%E2%80%93odd_rule) to determine the region to fill. This operator shall have the same effect as the sequence `h B*`.

#### Source
* [PDF Specification / Table 60 – Path-Painting Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987646)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2851](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2851)

### 28: endPath (`n`)
#### Operands
*none*

#### PDF Specification
End the path object without filling or stroking it. This operator shall be a path-painting no-op, used primarily for  the side effect of changing the current clipping path.

#### Source
* [PDF Specification / Table 60 – Path-Painting Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987646)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2852](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2852)

### 29: clip (`W`)
#### Operands
*none*

#### PDF Specification
Modify the current clipping path by intersecting it with the current path, using the [nonzero winding number rule](http://en.wikipedia.org/wiki/Nonzero-rule) to determine which regions lie inside the clipping path. 

#### Source
* [PDF Specification / Table 61 – Clipping Path Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.1850168)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2855](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2855)

### 30: eoClip (`W*`)
#### Operands
*none*

#### PDF Specification
Modify the current clipping path by intersecting it with the current path, using the [even-odd rule](http://en.wikipedia.org/wiki/Even%E2%80%93odd_rule) to determine which regions lie inside the clipping path.

#### Source
* [PDF Specification / Table 61 – Clipping Path Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.1850168)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2856](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2856)

### 31: beginText (`BT`)
#### Operands
*none*

#### PDF Specification
Begin a text object, initializing the text matrix and the text line matrix to the identity matrix. Text objects shall not be nested; a second **`BT`** shall not appear before an **`ET`**. 

#### Source
* [PDF Specification / Table 107 – Text object operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1958458)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2859](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2859)

### 32: endText (`ET`)
#### Operands
*none*

#### PDF Specification
End a text object, discarding the text matrix.

#### Source
* [PDF Specification / Table 107 – Text object operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1958458)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2860](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2860)

### 33: setCharSpacing (`Tc`)
#### Operands
* `charSpace`: *`number`*

#### PDF Specification
Set the character spacing to `charSpace`, which shall be a number expressed in unscaled text space units. Character spacing shall be used by the **`Tj`** *([`showText`](#44-showText-tj))*, **`TJ`** *([`showSpacedText`](#45-showspacedtext-tj))*, and **`'`** *([`nextLineShowText`](#46-nextlineshowtext-))* operators. Initial value: `0`. 

#### Source
* [PDF Specification / Table 105 – Text state operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1958454)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2861](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2861)

### 34: setWordSpacing (`Tw`)
#### Operands
* `wordSpace`: *`number`*

#### PDF Specification
Set the word spacing to `wordSpace`, which shall be a number expressed in unscaled text space units. Word spacing shall be used by the **`Tj`** *([`showText`](#44-showText-tj))*, **`TJ`** *([`showSpacedText`](#45-showspacedtext-tj))*, and **`'`** *([`nextLineShowText`](#46-nextlineshowtext-))* operators. Initial value: `0`.

#### Source
* [PDF Specification / Table 105 – Text state operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1958454)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2862](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2862)

### 35: setHScale (`Tz`)
#### Operands
* `scale`: *`number`*

#### PDF Specification
Set the horizontal scaling to (`scale` ÷ 100). `scale` shall be a number specifying the percentage of the normal width. Initial value: `100` (normal width). 

#### Source
* [PDF Specification / Table 105 – Text state operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1958454)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2863](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2863)

### 36: setLeading (`TL`)
#### Operands
* `leading`: *`number`*

#### PDF Specification
Set the text leading to `leading`, which shall be a number expressed in unscaled text space units. Text leading shall be used only by the **`T*`** *([`nextLine`](#43-nextline-t))*, **`'`** *([`nextLineShowText`](#46-nextlineshowtext-))*, and **`"`** *([`nextLineSetSpacingShowText`](#47-nextlinesetspacingshowtext-))* operators. Initial value: `0`. 

#### Source
* [PDF Specification / Table 105 – Text state operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1958454)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2864](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2864)

### 37: setFont (`Tf`)
#### Operands
* `font`: *`string`*
* `size`: *`number`*

#### PDF Specification
Set the text font to `font` and the text font size to `size`. `font` shall be the name of a font resource in the **Font** subdictionary of the current resource dictionary; `size` shall be a number representing a scale factor. There is no initial value for either `font` or `size`; they shall be specified explicitly by using **`Tf`** before any text is shown.

#### Source
* [PDF Specification / Table 105 – Text state operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1958454)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2865](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2865)

### 38: setTextRenderingMode (`Tr`)
#### Operands
* `render`: ?

#### PDF Specification
Set the text rendering mode to `render`, which shall be an integer. Initial value: `0`.

#### Source
* [PDF Specification / Table 105 – Text state operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1958454)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2866](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2866)

### 39: setTextRise (`Ts`)
#### Operands
* `rise`: ?

#### PDF Specification
Set the text rise to `rise`, which shall be a number expressed in unscaled text space units. Initial value: `0`.

#### Source
* [PDF Specification / Table 105 – Text state operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1958454)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2868](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2868)

### 40: moveText (`Td`)
#### Operands
* `tx`: *`number`*
* `ty`: *`number`*

#### PDF Specification
Move to the start of the next line, offset from the start of the current line by (`tx`, `ty`). `tx` and `ty` shall denote numbers expressed in unscaled text space units. More precisely, this operator shall perform these assignments: 
```
           ⎡ 1  0  0 ⎤
Tm = Tlm = ⎢ 0  1  0 ⎥ ⨯ Tlm
           ⎣ tx ty 1 ⎦

Tm  = text matrix
Tlm = text line matrix
```

#### Source
* [PDF Specification / Table 108 – Text-positioning operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1849331)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2869](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2869)

### 41: setLeadingMoveText (`TD`)
#### Operands
* `tx`: *`number`*
* `ty`: *`number`*

#### PDF Specification
Move to the start of the next line, offset from the start of the current line by (`tx`, `ty`). As a side effect, this operator shall set the leading parameter in the text state. This operator shall have the same effect as this code:
```
−ty TL
tx ty Td
```

#### Source
* [PDF Specification / Table 108 – Text-positioning operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1849331)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2870](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2870)

### 42: setTextMatrix (`Tm`)
#### Operands
* `a`: *`number`* - horizontal scaling
* `b`: *`number`* - horizontal skewing
* `c`: *`number`* - vertical skewing
* `d`: *`number`* - vertical scaling
* `e`: *`number`* - horizontal moving
* `f`: *`number`* - vertical moving

#### PDF Specification
Set the text matrix, **Tm**, and the text line matrix, **Tlm**:
```
           ⎡ a d 0 ⎤
Tm = Tlm = ⎢ b e 0 ⎥
           ⎣ c f 1 ⎦

Tm  = text matrix
Tlm = text line matrix
```
The operands shall all be numbers, and the initial value for **Tm** and **Tlm** shall be the identity matrix, `[ 1  0  0  1  0  0 ]`. Although the operands specify a matrix, they shall be passed to **Tm** as six separate numbers, not as an array. 

The matrix specified by the operands shall not be concatenated onto the current text matrix, but shall replace it. 

#### Source
* [PDF Specification / Table 108 – Text-positioning operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1849331)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2871](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2871)

### 43: nextLine (`T*`)
#### Operands
*none*

#### PDF Specification
Move to the start of the next line. This operator has the same effect as the code 
```
0 -Tl Td
```
where `Tl` denotes the current leading parameter in the text state. The negative of `Tl` is used here because `Tl` is the text leading expressed as a positive number. Going to the next line entails decreasing the y coordinate.

#### Source
* [PDF Specification / Table 108 – Text-positioning operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1849331)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2872](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2872)

### 44: showText (`Tj`)
#### Operands
Operands differ between the PDF specification and the implementation in PDF.js.
* `string`: *`string`* (PDF Specification)
* `glyphs`: *`array`* (implementation in PDF.js)

#### PDF Specification
Show a text string.

#### Source
* [PDF Specification / Table 109 – Text-showing operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1898300)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2873](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2873)

### 45: showSpacedText (`TJ`)
#### Operands
* `array`: ?

#### PDF Specification
Show one or more text strings, allowing individual glyph positioning. Each element of `array` shall be either a string or a number. If the element is a string, this operator shall show the string. If it is a number, the operator shall adjust the text position by that amount; that is, it shall translate the text matrix, **Tm**. The number shall be expressed in thousandths of a unit of text space. This amount shall be subtracted from the current horizontal or vertical coordinate, depending on the writing mode. In the default coordinate system, a positive adjustment has the  effect of moving the next glyph painted either to the left or down by the given amount.

#### Source
* [PDF Specification / Table 109 – Text-showing operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1898300)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2874](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2874)

### 46: nextLineShowText (`'`)
#### Operands
Operands differ between the PDF specification and the implementation in PDF.js.
* `string`: *`string`* (PDF Specification)
* `glyphs`: *`array`* (implementation in PDF.js)

#### PDF Specification
Move to the next line and show a text string. This operator shall have the same effect as the code
```
T*
string Tj
```

#### Source
* [PDF Specification / Table 109 – Text-showing operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1898300)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2875](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2875)

### 47: nextLineSetSpacingShowText (`"`)
#### Operands
Operands differ between the PDF specification and the implementation in PDF.js.
* `aw`: *`number`*
* `ac`: *`number`*
* `string`: *`string`* (PDF Specification)
* `glyphs`: *`array`* (implementation in PDF.js)

#### PDF Specification
Move to the next line and show a text string, using `aw` as the word spacing and `ac` as the character spacing (setting the corresponding parameters in the text state). `aw` and `ac` shall be numbers expressed in unscaled text space units. This operator shall have the same effect as this code:
```
aw Tw
ac Tc
string '
```

#### Source
* [PDF Specification / Table 109 – Text-showing operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1898300)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2876-L2877](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2876-L2877)

### 48: setCharWidth (`d0`)
#### Operands
* `wx`: *`number`*
* `wy`: *`number`*

#### PDF Specification
Set width information for the glyph and declare that the glyph description specifies both its shape and its colour.

`wx` denotes the horizontal displacement in the glyph coordinate system; it shall be consistent with the corresponding width in the font’s **Widths** array. `wy` shall be `0`.

This operator shall only be permitted in a content stream appearing in a Type 3 font’s **CharProcs** dictionary. It is  typically used only if the glyph description executes operators to set the colour explicitly. 

#### Source
* [PDF Specification / Table 113 – Type 3 font operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1853938)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2880](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2880)

### 49: setCharWidthAndBounds (`d1`)
#### Operands
* `wx`: *`number`*
* `wy`: *`number`*
* `llx`: ?
* `lly`: ?
* `urx`: ?
* `ury`: ?

#### Source
* [PDF Specification / Table 113 – Type 3 font operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G8.1853938)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2881](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2881)

### 50: setStrokeColorSpace (`CS`)
#### Operands
* `name`: *`string`*

#### Source
* [PDF Specification / Table 74 – Colour Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793574)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2885](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2885)

### 51: setFillColorSpace (`cs`)
#### Operands
* `name`: *`string`*

#### Source
* [PDF Specification / Table 74 – Colour Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793574)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2886](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2886)

### 52: setStrokeColor (`SC`)
#### Operands
For gray color spaces (DeviceGray, CalGray or Indexed):
* `scale`: *`number`*

For RGB color spaces (DeviceRGB, CalRGB or Lab):
* `r`: *`number`*
* `g`: *`number`*
* `b`: *`number`*

For CMYK color spaces (DeviceCMYK):
* `c`: *`number`*
* `m`: *`number`*
* `y`: *`number`*
* `k`: *`number`*

#### Source
* [PDF Specification / Table 74 – Colour Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793574)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2887](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2887)

### 53: setStrokeColorN (`SCN`)
#### Operands
* `c1` ... `cn` (`n` <= 32)
or
* `c1` ... `cn` (`n` <= 32)
* `name`

#### Source
* [PDF Specification / Table 74 – Colour Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793574)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2888](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2888)

### 54: setFillColor (`sc`)
#### Operands
* ?
* ?
* ?
* ? // variable

#### Source
* [PDF Specification / Table 74 – Colour Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793574)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2889](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2889)

### 55: setFillColorN (`scn`)
#### Operands
* ? (33) // variable

#### Source
* [PDF Specification / Table 74 – Colour Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793574)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2890](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2890)

### 56: setStrokeGray (`G`)
#### Operands
* `gray`: *`number`* - value between `0.0` (black) and `1.0` (white)

#### Source
* [PDF Specification / Table 74 – Colour Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793574)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2891](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2891)

### 57: setFillGray (`g`)
#### Operands
* `gray`: *`number`* - value between `0.0` (black) and `1.0` (white).

#### Source
* [PDF Specification / Table 74 – Colour Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793574)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2892](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2892)

### 58: setStrokeRGBColor (`RG`)
#### Operands
* `r`: *`number`* - value between `0.0` (minimum intensity) and `1.0` (maximum intensity)
* `g`: *`number`* - value between `0.0` (minimum intensity) and `1.0` (maximum intensity)
* `b`: *`number`* - value between `0.0` (minimum intensity) and `1.0` (maximum intensity)

#### Source
* [PDF Specification / Table 74 – Colour Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793574)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2893](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2893)

### 59: setFillRGBColor (`rg`)
#### Operands
* `r`: *`number`* - value between `0.0` (minimum intensity) and `1.0` (maximum intensity)
* `g`: *`number`* - value between `0.0` (minimum intensity) and `1.0` (maximum intensity)
* `b`: *`number`* - value between `0.0` (minimum intensity) and `1.0` (maximum intensity)

#### Source
* [PDF Specification / Table 74 – Colour Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793574)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2894](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2894)

### 60: setStrokeCMYKColor (`K`)
#### Operands
* `c`: *`number`* - value between `0.0` (zero concentration) and `1.0` (maximum concentration)
* `m`: *`number`* - value between `0.0` (zero concentration) and `1.0` (maximum concentration)
* `y`: *`number`* - value between `0.0` (zero concentration) and `1.0` (maximum concentration)
* `k`: *`number`* - value between `0.0` (zero concentration) and `1.0` (maximum concentration)

#### Source
* [PDF Specification / Table 74 – Colour Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793574)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2895](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2895)

### 61: setFillCMYKColor (`k`)
#### Operands
* `c`: *`number`* - value between `0.0` (zero concentration) and `1.0` (maximum concentration)
* `m`: *`number`* - value between `0.0` (zero concentration) and `1.0` (maximum concentration)
* `y`: *`number`* - value between `0.0` (zero concentration) and `1.0` (maximum concentration)
* `k`: *`number`* - value between `0.0` (zero concentration) and `1.0` (maximum concentration)

#### Source
* [PDF Specification / Table 74 – Colour Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3793574)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2896](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2896)

### 62: shadingFill (`sh`)
#### Operands
* `name`: *`string`*

#### Source
* [PDF Specification / Table 77 – Shading Operator](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.1851121)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2899](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2899)

### 63: beginInlineImage (`BI`)
#### Operands
*none*

#### PDF Specification
Begin an inline image object.

#### Source
* [PDF Specification / Table 92 – Inline Image Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.1852045)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2902](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2902)

### 64: beginImageData (`ID`)
#### Operands
*none*

#### PDF Specification
Begin the image data for an inline image object.

#### Source
* [PDF Specification / Table 92 – Inline Image Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.1852045)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2903](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2903)

### 65: endInlineImage (`EI`)
#### Operands
*none*

#### PDF Specification
End an inline image object.

#### Source
* [PDF Specification / Table 92 – Inline Image Operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.1852045)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2904](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2904)

### 66: paintXObject (`Do`)
#### Operands
* `name`: ?

#### PDF Specification
Paint the specified XObject. The operand `name` shall appear as a key in the **XObject** subdictionary of the current resource dictionary. The associated value shall be a stream whose **Type** entry, if present, is **XObject**. The effect of **`Do`** depends on the value of the XObject’s Subtype entry, which may be **Image**, **Form**, or **PS** (PostScript XObject). 

#### Source
* [PDF Specification / Table 87 – XObject Operator](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G7.3987702)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2907](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2907)

### 67: markPoint (`MP`)
#### Operands
* `tag`: ?

#### Source
* [PDF Specification / Table 320 – Marked-content operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G13.2438921)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2908](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2908)

### 68: markPointProps (`DP`)
#### Operands
* `tag`: ?
* `properties`: ?

#### Source
* [PDF Specification / Table 320 – Marked-content operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G13.2438921)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2909](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2909)

### 69: beginMarkedContent (`BMC`)
#### Operands
* `tag`: ?

#### Source
* [PDF Specification / Table 320 – Marked-content operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G13.2438921)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2910](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2910)

### 70: beginMarkedContentProps (`BDC`)
#### Operands
* `tag`: ?
* `properties`: ?

#### Source
* [PDF Specification / Table 320 – Marked-content operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G13.2438921)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2911-L2912](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2911-L2912)

### 71: endMarkedContent (`EMC`)
#### Operands
*none*

#### Source
* [PDF Specification / Table 320 – Marked-content operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G13.2438921)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2913](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2913)

### 72: beginCompat (`BX`)
#### Operands
*none*

#### Source
* [PDF Specification / Table 32 – Compatibility operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G6.1913072)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2916](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2916)

### 73: endCompat (`EX`)
#### Operands
*none*

#### Source
* [PDF Specification / Table 32 – Compatibility operators](https://www.adobe.com/content/dam/acom/en/devnet/pdf/PDF32000_2008.pdf#G6.1913072)
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L2917](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L2917)

### 74: paintFormXObjectBegin
#### Operands
* `matrix`: ?
* `bbox`: ?

#### Source
[https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L331](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L331)

### 75: paintFormXObjectEnd
#### Operands
*none*

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L340](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L340)

### 76: beginGroup
#### Operands
* `groupOptions`: `object`
  * `matrix`: ?
  * `bbox`: ?
  * `smask`: ?
  * `isolated`: `boolean` (default value: `false`)
  * `knockout`: `boolean` (default value: `false`)

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L328](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L328)

### 77: endGroup
#### Operands
* `groupOptions`: *`object`*
  * `matrix`: ?
  * `bbox`: ?
  * `smask`: ?
  * `isolated`: *`boolean`* (default value: `false`)
  * `knockout`: *`boolean`* (default value: `false`)

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L343](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L343)

### 78: beginAnnotations
#### Operands
*none*

#### Source
* * [https://github.com/mozilla/pdf.js/.../src/core/document.js#L247](https://github.com/mozilla/pdf.js/blob/88e87ce240676d4be4ab52f6d9ed567a1827d5fa/src/core/document.js#L247)

### 79: endAnnotations
#### Operands
*none*

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/document.js#L251](https://github.com/mozilla/pdf.js/blob/88e87ce240676d4be4ab52f6d9ed567a1827d5fa/src/core/document.js#L251)

### 80: beginAnnotation
#### Operands
* `rect`: ?
* `transform`: ?
* `matrix`: ?

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/annotation.js#L450](https://github.com/mozilla/pdf.js/blob/ff2df9c5b64a700e06694ff3375cd54c10ccedf4/src/core/annotation.js#L450)

### 81: endAnnotation
#### Operands
*none*

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/annotation.js#L457](https://github.com/mozilla/pdf.js/blob/ff2df9c5b64a700e06694ff3375cd54c10ccedf4/src/core/annotation.js#L457)

### 82: paintJpegXObject
* `objId`: ?
* `w`: *`number`*
* `h`: *`number`*

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L441](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L441)

### 83: paintImageMaskXObject
#### Operands
* `imgData`: ?

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L390](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L390)

### 84: paintImageMaskXObjectGroup
#### Operands
* `images`: ?

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/operator_list.js#L251](https://github.com/mozilla/pdf.js/blob/160ca551633cd7901ee68ae6ef748ec7d6ec0aed/src/core/operator_list.js#L251)

### 85: paintImageXObject
#### Operands
* `objId`: ?
* `w`: *`number`*
* `h`: *`number`*

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L499](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L499)

### 86: paintInlineImageXObject
#### Operands
* `imgData`: ?

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L417](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L417)

### 87: paintInlineImageXObjectGroup
* `imgData`: ?
* `map`: ?

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/operator_list.js#L148](https://github.com/mozilla/pdf.js/blob/160ca551633cd7901ee68ae6ef748ec7d6ec0aed/src/core/operator_list.js#L148)

### 88: paintImageXObjectRepeat
* `objId`: ?
* `scaleX`: ?
* `scaleY`: ?
* `positions`: ?

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/operator_list.js#L337](https://github.com/mozilla/pdf.js/blob/160ca551633cd7901ee68ae6ef748ec7d6ec0aed/src/core/operator_list.js#L337)

### 89: paintImageMaskXObjectRepeat
* `imgData`: ?
* `scaleX`: ?
* `scaleY`: ?
* `positions`: ?

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/operator_list.js#L236](https://github.com/mozilla/pdf.js/blob/160ca551633cd7901ee68ae6ef748ec7d6ec0aed/src/core/operator_list.js#L236)

### 90: paintSolidColorImageMask
#### Operands
*none*

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/operator_list.js#L45](https://github.com/mozilla/pdf.js/blob/160ca551633cd7901ee68ae6ef748ec7d6ec0aed/src/core/operator_list.js#L45)

### 91: constructPath
#### Operands
* `ops`: ?
* `args`: ?

#### Source
* [https://github.com/mozilla/pdf.js/.../src/core/evaluator.js#L881](https://github.com/mozilla/pdf.js/blob/842e9206c059d36b9592e1e1b214985da6b57170/src/core/evaluator.js#L881)
