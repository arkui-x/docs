OffscreenCanvasRenderingContext2D

Use **OffscreenCanvasRenderingContext2D** to draw rectangles, images, and text offscreen onto a canvas. Drawing offscreen onto a canvas is a process where content to draw onto the canvas is first drawn in the buffer, and then converted into a picture, and finally the picture is drawn on the canvas. This process increases the drawing efficiency.

>  **NOTE**
>
>  The APIs of this module are supported since API version 8. Updates will be marked with a superscript to indicate their earliest API version.



## APIs

OffscreenCanvasRenderingContext2D(width: number, height: number, settings?: RenderingContextSettings)

Since API version 9, this API is supported in ArkTS widgets.

**Parameters**

| Name     | Type                                    | Mandatory  | Description                          |
| -------- | ---------------------------------------- | ---- | ------------------------------ |
| width    | number                                   | Yes   | Width of the offscreen canvas.                       |
| height   | number                                   | Yes   | Height of the offscreen canvas.                       |
| settings | [RenderingContextSettings](ts-canvasrenderingcontext2d.md#renderingcontextsettings) | No   | See RenderingContextSettings.|


## Attributes

| Name                                                  | Type                  | Description                                                  |
| ----------------------------------------------------- | --------------------- | ------------------------------------------------------------ |
| lineWidth                                             | number                | Line width.<br>Since API version 9, this API is supported in ArkTS widgets. |
| lineCap                                               | CanvasLineCap         | Style of the line endpoints. The options are as follows:<br>- **butt**: The endpoints of the line are squared off.<br>- **round**: The endpoints of the line are rounded.<br>- **square**: The endpoints of the line are squared off, and each endpoint has added a rectangle whose length is the same as the line thickness and whose width is half of the line thickness.<br>- Default value: **'butt'**<br>Since API version 9, this API is supported in ArkTS widgets. |
| lineJoin                                              | CanvasLineJoin        | Style of the shape used to join line segments. The options are as follows:<br>- **round**: The intersection is a sector, whose radius at the rounded corner is equal to the line width.<br>- **bevel**: The intersection is a triangle. The rectangular corner of each line is independent.<br>- **miter**: The intersection has a miter corner by extending the outside edges of the lines until they meet. You can view the effect of this attribute in **miterLimit**.<br>- Default value: **'miter'**<br>Since API version 9, this API is supported in ArkTS widgets. |
| miterLimit                                            | number                | Maximum miter length. The miter length is the distance between the inner corner and the outer corner where two lines meet.<br>- Default value: **10**<br>Since API version 9, this API is supported in ArkTS widgets. |
| [font](#font)                                         | string                | Font style.<br>Syntax: ctx.font='font-size font-family'<br>- (Optional) **font-size**: font size and row height. The unit can only be pixels.<br>(Optional) **font-family**: font family.<br>Syntax: ctx.font='font-style font-weight font-size font-family'<br>- (Optional) **font-style**: font style. Available values are **normal** and **italic**.<br>- (Optional) **font-weight**: font weight. Available values are as follows: **normal**, **bold**, **bolder**, **lighter**, **100**, **200**, **300**, **400**, **500**, **600**, **700**, **800**, **900**.<br>- (Optional) **font-size**: font size and line height. The unit must be specified and can only be px or vp.<br>- (Optional) **font-family**: font family. Available values are **sans-serif**, **serif**, and **monospace**.<br>Default value: **'normal normal 14px sans-serif'**<br>Since API version 9, this API is supported in ArkTS widgets. |
| textBaseline                                          | CanvasTextBaseline    | Horizontal alignment mode of text. Available values are as follows:<br>- **alphabetic**: The text baseline is the normal alphabetic baseline.<br>- **top**: The text baseline is on the top of the text bounding box.<br>- **hanging**: The text baseline is a hanging baseline over the text.<br>- **middle**: The text baseline is in the middle of the text bounding box.<br>**'ideographic'**: The text baseline is the ideographic baseline. If a character exceeds the alphabetic baseline, the ideographic baseline is located at the bottom of the excess character.<br>- **bottom**: The text baseline is at the bottom of the text bounding box. Its difference from the ideographic baseline is that the ideographic baseline does not consider letters in the next line.<br>- Default value: **'alphabetic'**<br>Since API version 9, this API is supported in ArkTS widgets. |
| globalAlpha                                           | number                | Opacity.<br>**0.0**: completely transparent.<br>**1.0**: completely opaque. |
| [lineDashOffset](#linedashoffset)                     | number                | Offset of the dashed line. The precision is float.<br>- Default value: **0.0**<br>Since API version 9, this API is supported in ArkTS widgets. |
| [globalCompositeOperation](#globalcompositeoperation) | string                | Composition operation type. Available values are as follows: **'source-over'**, **'source-atop'**, **'source-in'**, **'source-out'**, **'destination-over'**, **'destination-atop'**, **'destination-in'**, **'destination-out'**, **'lighter'**, **'copy'**, and **'xor'**.<br>- Default value: **'source-over'**<br>Since API version 9, this API is supported in ArkTS widgets. |
| shadowBlur                                            | number                | Blur level during shadow drawing. A larger value indicates a more blurred effect. The precision is float.<br>- Default value: **0.0**<br>Since API version 9, this API is supported in ArkTS widgets. |
| shadowColor                                           | string                | Shadow color.<br>Since API version 9, this API is supported in ArkTS widgets. |
| shadowOffsetX                                         | number                | X-axis shadow offset relative to the original object.<br>Since API version 9, this API is supported in ArkTS widgets. |
| shadowOffsetY                                         | number                | Y-axis shadow offset relative to the original object.<br>Since API version 9, this API is supported in ArkTS widgets. |
| [imageSmoothingEnabled](#imagesmoothingenabled)       | boolean               | Whether to adjust the image smoothness during image drawing. The value **true** means to enable this feature, and **false** means the opposite.<br>- Default value: **true**<br>Since API version 9, this API is supported in ArkTS widgets. |
| [imageSmoothingQuality](#imagesmoothingquality)       | ImageSmoothingQuality | Quality of image smoothing. This attribute works only when **imageSmoothingEnabled** is set to **true**. Available values are as follows:<br>- **'low'**: low quality.<br>- **'medium'**: medium quality.<br>- **'high'**: high quality.<br>Default value: **'low'**<br>Since API version 9, this API is supported in ArkTS widgets. |
| [direction](#direction)                               | CanvasDirection       | Text direction used for drawing text. Available values are as follows:<br>- **'inherit'**: The text direction is inherited from the **\<Canvas>** component.<br>- **'ltr'**: The text direction is from left to right.<br>- **'rtl'**: The text direction is from right to left.<br>Default value: **'inherit'**<br>Since API version 9, this API is supported in ArkTS widgets. |
| [filter](#filter)                                     | string                | Filter effect. Available values are as follows:<br>- **'none'**: no filter effect.<br>- **'blur'**: applies the Gaussian blur for the image.<br>- **'brightness'**: applies a linear multiplication to the image to make it look brighter or darker.<br>- **'contrast'**: adjusts the image contrast.<br>- **'grayscale'**: converts the image to a grayscale image.<br>- **'hue-rotate'**: applies hue rotation to the image.<br>- **'invert'**: inverts the input image.<br>- **'opacity'**: sets the opacity of the image.<br>- **'saturate'**: sets the saturation of the image.<br>- **'sepia'**: converts the image to dark brown.<br>Default value: **'none'**<br>Since API version 9, this API is supported in ArkTS widgets. |

> **NOTE**
> For **fillStyle**, **shadowColor**, and **strokeStyle**, the value format of the string type is 'rgb(255, 255, 255)', 'rgba(255, 255, 255, 1.0)', '\#FFFFFF'.


### globalCompositeOperation

| Name              | Description                      |
| ---------------- | ------------------------ |
| source-over      | Displays the new drawing above the existing drawing. This attribute is used by default.  |
| source-atop      | Displays the new drawing on the top of the existing drawing.       |
| source-in        | Displays the new drawing inside the existing drawing.        |
| source-out       | Displays part of the new drawing that is outside of the existing drawing.       |
| destination-over | Displays the existing drawing above the new drawing.       |
| destination-atop | Displays the existing drawing on the top of the new drawing.       |
| destination-in   | Displays the existing drawing inside the new drawing.        |
| destination-out  | Displays the existing drawing outside the new drawing.        |
| lighter          | Displays both the new and existing drawing.         |
| copy             | Displays the new drawing and neglects the existing drawing.       |
| xor              | Combines the new drawing and existing drawing using the XOR operation.|


### fillRect

fillRect(x: number, y: number, w: number, h: number): void

Fills a rectangle on the canvas.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name    | Type    | Mandatory  | Default Value | Description           |
| ------ | ------ | ---- | ---- | ------------- |
| x      | number | Yes   | 0    | X-coordinate of the upper left corner of the rectangle.|
| y      | number | Yes   | 0    | Y-coordinate of the upper left corner of the rectangle.|
| width  | number | Yes   | 0    | Width of the rectangle.     |
| height | number | Yes   | 0    | Height of the rectangle.     |


### strokeRect

strokeRect(x: number, y: number, w: number, h: number): void

Draws an outlined rectangle on the canvas.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name    | Type    | Mandatory  | Default Value | Description          |
| ------ | ------ | ---- | ---- | ------------ |
| x      | number | Yes   | 0    | X-coordinate of the upper left corner of the rectangle.|
| y      | number | Yes   | 0    | Y-coordinate of the upper left corner of the rectangle.|
| width  | number | Yes   | 0    | Width of the rectangle.    |
| height | number | Yes   | 0    | Height of the rectangle.    |


### clearRect

clearRect(x: number, y: number, w: number, h: number): void

Clears the content in a rectangle on the canvas.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name    | Type    | Mandatory  | Default Value | Description           |
| ------ | ------ | ---- | ---- | ------------- |
| x      | number | Yes   | 0    | X-coordinate of the upper left corner of the rectangle.|
| y      | number | Yes   | 0    | Y-coordinate of the upper left corner of the rectangle.|
| width  | number | Yes   | 0    | Width of the rectangle.     |
| height | number | Yes   | 0    | Height of the rectangle.     |


### fillText

fillText(text: string, x: number, y: number, maxWidth?: number): void

Draws filled text on the canvas.

Since API version 9, this API is supported in ArkTS widgets.

**Parameters**

| Name      | Type    | Mandatory  | Default Value | Description             |
| -------- | ------ | ---- | ---- | --------------- |
| text     | string | Yes   | ""   | Text to draw.     |
| x        | number | Yes   | 0    | X-coordinate of the lower left corner of the text.|
| y        | number | Yes   | 0    | Y-coordinate of the lower left corner of the text.|
| maxWidth | number | No   | -    | Maximum width allowed for the text.   |


### strokeText

strokeText(text: string, x: number, y: number): void

Draws a text stroke on the canvas.

Since API version 9, this API is supported in ArkTS widgets.

**Parameters**

| Name      | Type    | Mandatory  | Default Value | Description             |
| -------- | ------ | ---- | ---- | --------------- |
| text     | string | Yes   | ""   | Text to draw.     |
| x        | number | Yes   | 0    | X-coordinate of the lower left corner of the text.|
| y        | number | Yes   | 0    | Y-coordinate of the lower left corner of the text.|
| maxWidth | number | No   | -    | Maximum width of the text to be drawn. |


### measureText

measureText(text: string): TextMetrics

Returns a **TextMetrics** object used to obtain the width of specified text.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name  | Type    | Mandatory  | Default Value | Description        |
| ---- | ------ | ---- | ---- | ---------- |
| text | string | Yes   | ""   | Text to be measured.|

 **Return value**

| Type         | Description                                      |
| ----------- | ---------------------------------------- |
| TextMetrics | **TextMetrics** object.<br>Since API version 9, this API is supported in ArkTS widgets.|

**TextMetrics** attributes

| Name                      | Type    | Description                                      |
| ------------------------ | ------ | ---------------------------------------- |
| width                    | number | Width of the text.                                 |
| height                   | number | Height of the text.                                 |
| actualBoundingBoxAscent  | number | Distance from the horizontal line specified by the **CanvasRenderingContext2D.textBaseline** attribute to the top of the bounding rectangle used to render the text. The current value is **0**.|
| actualBoundingBoxDescent | number | Distance from the horizontal line specified by the **CanvasRenderingContext2D.textBaseline** attribute to the bottom of the bounding rectangle used to render the text. The current value is **0**.|
| actualBoundingBoxLeft    | number | Distance parallel to the baseline from the alignment point determined by the **CanvasRenderingContext2D.textAlign** attribute to the left side of the bounding rectangle of the text. The current value is **0**.|
| actualBoundingBoxRight   | number | Distance parallel to the baseline from the alignment point determined by the **CanvasRenderingContext2D.textAlign** attribute to the right side of the bounding rectangle of the text. The current value is **0**.|
| alphabeticBaseline       | number | Distance from the horizontal line specified by the **CanvasRenderingContext2D.textBaseline** attribute to the alphabetic baseline of the line box. The current value is **0**.|
| emHeightAscent           | number | Distance from the horizontal line specified by the **CanvasRenderingContext2D.textBaseline** attribute to the top of the em square in the line box. The current value is **0**.|
| emHeightDescent          | number | Distance from the horizontal line specified by the **CanvasRenderingContext2D.textBaseline** attribute to the bottom of the em square in the line box. The current value is **0**.|
| fontBoundingBoxAscent    | number | Distance from the horizontal line specified by the **CanvasRenderingContext2D.textBaseline** attribute to the top of the highest bounding rectangle of all the fonts used to render the text. The current value is **0**.|
| fontBoundingBoxDescent   | number | Distance from the horizontal line specified by the **CanvasRenderingContext2D.textBaseline** attribute to the bottom of the bounding rectangle of all the fonts used to render the text. The current value is **0**.|
| hangingBaseline          | number | Distance from the horizontal line specified by the **CanvasRenderingContext2D.textBaseline** attribute to the hanging baseline of the line box. The current value is **0**.|
| ideographicBaseline      | number | Distance from the horizontal line indicated by the **CanvasRenderingContext2D.textBaseline** attribute to the ideographic baseline of the line box. The current value is **0**.|


### stroke

stroke(path?: Path2D): void

Strokes a path.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name  | Type                                      | Mandatory  | Default Value | Description          |
| ---- | ---------------------------------------- | ---- | ---- | ------------ |
| path | [Path2D](ts-components-canvas-path2d.md) | No   | null | A **Path2D** path to draw.|


### beginPath

beginPath(): void

Creates a drawing path.

Since API version 9, this API is supported in ArkTS widgets.


### createPattern

createPattern(image: ImageBitmap, repetition: string | null): CanvasPattern | null

Creates a pattern for image filling based on a specified source image and repetition mode.

Since API version 9, this API is supported in ArkTS widgets.

**Parameters**

| Name        | Type                                      | Mandatory  | Default Value | Description                                      |
| ---------- | ---------------------------------------- | ---- | ---- | ---------------------------------------- |
| image      | [ImageBitmap](ts-components-canvas-imagebitmap.md) | Yes   | null | Source image. For details, see **ImageBitmap**.                 |
| repetition | string                                   | Yes   | ""  | Repetition mode. The value can be **'repeat'**, **'repeat-x'**, **'repeat-y'**, **'no-repeat'**, **'clamp'**, or **'mirror'**.|

**Return value**

| Type                                      | Description                     |
| ---------------------------------------- | ----------------------- |
| [CanvasPattern](ts-components-canvas-canvaspattern.md#canvaspattern) | Created pattern for image filling based on a specified source image and repetition mode.|


### rect

rect(x: number, y: number, w: number, h: number): void

Creates a rectangle on the canvas.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name  | Type    | Mandatory  | Default Value | Description           |
| ---- | ------ | ---- | ---- | ------------- |
| x    | number | Yes   | 0    | X-coordinate of the upper left corner of the rectangle.|
| y    | number | Yes   | 0    | Y-coordinate of the upper left corner of the rectangle.|
| w    | number | Yes   | 0    | Width of the rectangle.     |
| h    | number | Yes   | 0    | Height of the rectangle.     |

### fill

fill(fillRule?: CanvasFillRule): void

Fills the area inside a closed path on the canvas.

Since API version 9, this API is supported in ArkTS widgets.

**Parameters**

| Name      | Type            | Mandatory  | Default Value      | Description                                      |
| -------- | -------------- | ---- | --------- | ---------------------------------------- |
| fillRule | CanvasFillRule | No   | "nonzero" | Rule by which to determine whether a point is inside or outside the area to fill.<br>The options are **"nonzero"** and **"evenodd"**.|


fill(path: Path2D, fillRule?: CanvasFillRule): void

Fills the area inside a closed path on the canvas.

Since API version 9, this API is supported in ArkTS widgets.

**Parameters**

| Name      | Type            | Mandatory  | Default Value      | Description                                      |
| -------- | -------------- | ---- | --------- | ---------------------------------------- |
| path     | Path2D         | Yes   |           | A **Path2D** path to fill.                             |
| fillRule | CanvasFillRule | No   | "nonzero" | Rule by which to determine whether a point is inside or outside the area to fill.<br>The options are **"nonzero"** and **"evenodd"**.|

### clip

clip(fillRule?: CanvasFillRule): void

Sets the current path to a clipping path.

Since API version 9, this API is supported in ArkTS widgets.

**Parameters**

| Name      | Type            | Mandatory  | Default Value      | Description                                      |
| -------- | -------------- | ---- | --------- | ---------------------------------------- |
| fillRule | CanvasFillRule | No   | "nonzero" | Rule by which to determine whether a point is inside or outside the area to clip.<br>The options are **"nonzero"** and **"evenodd"**.|


clip(path:Path2D, fillRule?: CanvasFillRule): void

Sets a closed path to a clipping path.

Since API version 9, this API is supported in ArkTS widgets.

**Parameters**

| Name      | Type            | Mandatory  | Default Value      | Description                                      |
| -------- | -------------- | ---- | --------- | ---------------------------------------- |
| path     | Path2D         | Yes   |           | A **Path2D** path to clip.                             |
| fillRule | CanvasFillRule | No   | "nonzero" | Rule by which to determine whether a point is inside or outside the area to clip.<br>The options are **"nonzero"** and **"evenodd"**.|

### filter

filter(filter: string): void

Sets a filter for the image on the canvas.

Since API version 9, this API is supported in ArkTS widgets.

**Parameters**

| Name    | Type    | Mandatory  | Default Value | Description                                      |
| ------ | ------ | ---- | ---- | ---------------------------------------- |
| filter | string | Yes   | -    | Functions that accept various filter effects. Available values are as follows:<br>- **'none'**: no filter effect.<br>- **'blur'**: applies the Gaussian blur for the image.<br>- **'brightness'**: applies a linear multiplication to the image to make it look brighter or darker.<br>- **'contrast'**: adjusts the image contrast.<br>- **'grayscale'**: converts the image to a grayscale image.<br>- **'hue-rotate'**: applies hue rotation to the image.<br>- **'invert'**: inverts the input image.<br>- **'opacity'**: sets the opacity of the image.<br>- **'saturate'**: sets the saturation of the image.<br>- **'sepia'**: converts the image to dark brown.<br>Default value: **'none'**|


### getTransform

getTransform(): Matrix2D

Obtains the current transformation matrix being applied to the context. This API is a void API.

Since API version 9, this API is supported in ArkTS widgets.

**Return value**

| Type                                      | Description   |
| ---------------------------------------- | ----- |
| [Matrix2D](ts-components-canvas-matrix2d.md#Matrix2D) | Matrix object.|

### resetTransform

resetTransform(): void

Resets the current transform to the identity matrix. This API is a void API.

Since API version 9, this API is supported in ArkTS widgets.


### direction

direction(direction: CanvasDirection): void

Sets the text direction for drawing text.

Since API version 9, this API is supported in ArkTS widgets.

### rotate

rotate(angle: number): void

Rotates a canvas clockwise around its coordinate axes.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name   | Type    | Mandatory  | Default Value | Description                                      |
| ----- | ------ | ---- | ---- | ---------------------------------------- |
| angle | number | Yes   | 0    | Clockwise rotation angle. You can use **Math.PI / 180** to convert the angle to a radian.|


### scale

scale(x: number, y: number): void

Scales the canvas based on scale factors.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name  | Type    | Mandatory  | Default Value | Description         |
| ---- | ------ | ---- | ---- | ----------- |
| x    | number | Yes   | 0    | Horizontal scale factor.|
| y    | number | Yes   | 0    | Vertical scale factor.|


### transform

transform(a: number, b: number, c: number, d: number, e: number, f: number): void

Defines a transformation matrix. To transform a graph, you only need to set parameters of the matrix. The coordinates of the graph are multiplied by the matrix values to obtain new coordinates of the transformed graph. You can use the matrix to implement multiple transform effects.

Since API version 9, this API is supported in ArkTS widgets.

> **NOTE**
> The following formulas calculate coordinates of the transformed graph. **x** and **y** represent coordinates before transformation, and **x'** and **y'** represent coordinates after transformation.
>
> - x' = scaleX \* x + skewY \* y + translateX
>
> - y' = skewX \* x + scaleY \* y + translateY

**Parameters**

| Name  | Type    | Mandatory  | Default Value | Description                  |
| ---- | ------ | ---- | ---- | -------------------- |
| a    | number | Yes   | 0    | X-axis scale.    |
| b    | number | Yes   | 0    | X-axis skew.     |
| c    | number | Yes   | 0    | Y-axis skew.     |
| d    | number | Yes   | 0    | Y-axis scale.    |
| e    | number | Yes   | 0    | X-axis translation.|
| f    | number | Yes   | 0    | Y-axis translation.|


### setTransform

setTransform(a: number, b: number, c: number, d: number, e: number, f: number): void

Resets the existing transformation matrix and creates a new transformation matrix by using the same parameters as the **transform()** API.

Since API version 9, this API is supported in ArkTS widgets.

**Parameters**

| Name  | Type    | Mandatory  | Default Value | Description                  |
| ---- | ------ | ---- | ---- | -------------------- |
| a    | number | Yes   | 0    | X-axis scale.    |
| b    | number | Yes   | 0    | X-axis skew.     |
| c    | number | Yes   | 0    | Y-axis skew.     |
| d    | number | Yes   | 0    | Y-axis scale.    |
| e    | number | Yes   | 0    | X-axis translation.|
| f    | number | Yes   | 0    | Y-axis translation.|


### translate

translate(x: number, y: number): void

Moves the origin of the coordinate system.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name  | Type    | Mandatory  | Default Value | Description      |
| ---- | ------ | ---- | ---- | -------- |
| x    | number | Yes   | 0    | X-axis translation.|
| y    | number | Yes   | 0    | Y-axis translation.|


### drawImage

drawImage(image: ImageBitmap | PixelMap, dx: number, dy: number): void

drawImage(image: ImageBitmap | PixelMap, dx: number, dy: number, dw: number, dh: number): void

drawImage(image: ImageBitmap | PixelMap, sx: number, sy: number, sw: number, sh: number, dx: number, dy: number, dw: number, dh: number):void

Draws an image on the canvas.

Since API version 9, this API is supported in ArkTS widgets, except that **PixelMap** objects are not supported.

 **Parameters**

| Name   | Type                                      | Mandatory  | Default Value | Description                           |
| ----- | ---------------------------------------- | ---- | ---- | ----------------------------- |
| image | [ImageBitmap](ts-components-canvas-imagebitmap.md) or [PixelMap](../apis/js-apis-image.md#pixelmap7)| Yes   | null | Image resource. For details, see **ImageBitmap** or **PixelMap**.|
| sx    | number                                   | No   | 0    | X-coordinate of the upper left corner of the rectangle used to crop the source image.         |
| sy    | number                                   | No   | 0    | Y-coordinate of the upper left corner of the rectangle used to crop the source image.         |
| sw    | number                                   | No   | 0    | Target width to crop the source image.               |
| sh    | number                                   | No   | 0    | Target height to crop the source image.               |
| dx    | number                                   | Yes   | 0    | X-coordinate of the upper left corner of the drawing area on the canvas.               |
| dy    | number                                   | Yes   | 0    | Y-coordinate of the upper left corner of the drawing area on the canvas.         |
| dw    | number                                   | No   | 0    | Width of the drawing area.                     |
| dh    | number                                   | No   | 0    | Height of the drawing area.                     |


### createImageData

createImageData(sw: number, sh: number): ImageData

Creates an **[ImageData](ts-components-canvas-imagedata.md)** object with the specified dimensions.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name  | Type    | Mandatory  | Default Value  | Description           |
| ---- | ------ | ---- | ---- | ------------- |
| sw   | number | Yes   | 0    | Width of the **ImageData** object.|
| sh   | number | Yes   | 0    | Height of the **ImageData** object.|


createImageData(imageData: ImageData): ImageData

Creates an **[ImageData](ts-components-canvas-imagedata.md)** object by copying an existing **ImageData** object.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name       | Type                                      | Mandatory  | Default Value  | Description              |
| --------- | ---------------------------------------- | ---- | ---- | ---------------- |
| imagedata | [ImageData](ts-components-canvas-imagedata.md) | Yes   | null | **ImageData** object to copy.|

 **Return value**

| Type                                      | Description           |
| ---------------------------------------- | ------------- |
| [ImageData](ts-components-canvas-imagedata.md) | New **ImageData** object.|

### getPixelMap

getPixelMap(sx: number, sy: number, sw: number, sh: number): PixelMap

Obtains the **[PixelMap](../apis/js-apis-image.md#pixelmap7)** object created with the pixels within the specified area on the canvas.

 **Parameters**

| Name  | Type    | Mandatory  | Default Value | Description             |
| ---- | ------ | ---- | ---- | --------------- |
| sx   | number | Yes   | 0    | X-coordinate of the upper left corner of the output area.|
| sy   | number | Yes   | 0    | Y-coordinate of the upper left corner of the output area.|
| sw   | number | Yes   | 0    | Width of the output area.    |
| sh   | number | Yes   | 0    | Height of the output area.    |

**Return value**

| Type                                      | Description          |
| ---------------------------------------- | ------------ |
| [PixelMap](../apis/js-apis-image.md#pixelmap7) | **PixelMap** object.|

### setPixelMap

setPixelMap(value?: PixelMap): void

Draws the input [PixelMap](../apis/js-apis-image.md#pixelmap7) object on the canvas.

 **Parameters**

| Name  | Type    | Mandatory  | Default Value | Description             |
| ---- | ------ | ---- | ---- | --------------- |
| sx   | number | Yes   | 0    | X-coordinate of the upper left corner of the output area.|
| sy   | number | Yes   | 0    | Y-coordinate of the upper left corner of the output area.|
| sw   | number | Yes   | 0    | Width of the output area.    |
| sh   | number | Yes   | 0    | Height of the output area.    |

**Return value**

| Type                                      | Description          |
| ---------------------------------------- | ------------ |
| [PixelMap](../apis/js-apis-image.md#pixelmap7) | **PixelMap** object.|


### getImageData

getImageData(sx: number, sy: number, sw: number, sh: number): ImageData

Obtains the **[ImageData](ts-components-canvas-imagedata.md)** object created with the pixels within the specified area on the canvas.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name  | Type    | Mandatory  | Default Value | Description             |
| ---- | ------ | ---- | ---- | --------------- |
| sx   | number | Yes   | 0    | X-coordinate of the upper left corner of the output area.|
| sy   | number | Yes   | 0    | Y-coordinate of the upper left corner of the output area.|
| sw   | number | Yes   | 0    | Width of the output area.    |
| sh   | number | Yes   | 0    | Height of the output area.    |

   **Return value**

| Type                                      | Description           |
| ---------------------------------------- | ------------- |
| [ImageData](ts-components-canvas-imagedata.md) | New **ImageData** object.|


### putImageData

putImageData(imageData: Object, dx: number | string, dy: number | string): void

putImageData(imageData: Object, dx: number | string, dy: number | string, dirtyX: number | string, dirtyY: number | string, dirtyWidth?: number | string, dirtyHeight: number | string): void

Puts an **[ImageData](ts-components-canvas-imagedata.md)** object onto a rectangular area on the canvas.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name         | Type                                      | Mandatory  | Default Value         | Description                           |
| ----------- | ---------------------------------------- | ---- | ------------ | ----------------------------- |
| imagedata   | Object                                   | Yes   | null         | **ImageData** object with pixels to put onto the canvas.           |
| dx          | number \| string<sup>10+</sup> | Yes   | 0            | X-axis offset of the rectangular area on the canvas.               |
| dy          | number \| string<sup>10+</sup> | Yes   | 0            | Y-axis offset of the rectangular area on the canvas.               |
| dirtyX      | number \| string<sup>10+</sup> | No   | 0            | X-axis offset of the upper left corner of the rectangular area relative to that of the source image.|
| dirtyY      | number \| string<sup>10+</sup> | No   | 0            | Y-axis offset of the upper left corner of the rectangular area relative to that of the source image.|
| dirtyWidth  | number \| string<sup>10+</sup> | No   | Width of the **ImageData** object| Width of the rectangular area to crop the source image.              |
| dirtyHeight | number \| string<sup>10+</sup> | No   | Height of the **ImageData** object| Height of the rectangular area to crop the source image.              |

### setLineDash

setLineDash(segments: number[]): void

Sets the dash line style.

Since API version 9, this API is supported in ArkTS widgets.

**Parameters**

| Name      | Type      | Description                 |
| -------- | -------- | ------------------- |
| segments | number[] | An array of numbers that specify distances to alternately draw a line and a gap.|


### getLineDash

getLineDash(): number[]

Obtains the dash line style.

Since API version 9, this API is supported in ArkTS widgets.

**Return value**

| Type      | Description                      |
| -------- | ------------------------ |
| number[] | An array describing the interval of alternate line segments and length of spacing.|

### toDataURL

toDataURL(type?: string, quality?: number): string

Generates a URL containing image display information.

Since API version 9, this API is supported in ArkTS widgets.

**Parameters**

| Name    | Type  | Mandatory  | Description                                      |
| ------- | ------ | ---- | ---------------------------------------- |
| type    | string | No   | Image format. The default value is **image/png**.           |
| quality | number | No   | Image quality, which ranges from 0 to 1, when the image format is **image/jpeg** or **image/webp**. If the set value is beyond the value range, the default value **0.92** is used.|

**Return value**

| Type    | Description       |
| ------ | --------- |
| string | Image URL.|


### imageSmoothingQuality

imageSmoothingQuality(quality: imageSmoothingQuality)

Sets the quality of image smoothing.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name     | Type                   | Description                                      |
| ------- | --------------------- | ---------------------------------------- |
| quality | imageSmoothingQuality | Quality of image smoothing.<br>- **'low'**: low quality.<br>- **'medium'**: medium quality.<br>- **'high'**: high quality.|

### transferToImageBitmap

transferToImageBitmap(): ImageBitmap

Creates an **ImageBitmap** object on the most recently rendered image of the **OffscreenCanvas**.

**Return value**

| Type                                      | Description             |
| ---------------------------------------- | --------------- |
| [ImageBitmap](ts-components-canvas-imagebitmap.md) | Pixel data rendered on the **OffscreenCanvas**.|

### restore

restore(): void

Restores the saved drawing context.

Since API version 9, this API is supported in ArkTS widgets.


### save

save(): void

Saves the current drawing context.

Since API version 9, this API is supported in ArkTS widgets.

### createLinearGradient

createLinearGradient(x0: number, y0: number, x1: number, y1: number): void

Creates a linear gradient.

Since API version 9, this API is supported in ArkTS widgets.

 **Parameters**

| Name  | Type    | Mandatory  | Default Value | Description      |
| ---- | ------ | ---- | ---- | -------- |
| x0   | number | Yes   | 0    | X-coordinate of the start point.|
| y0   | number | Yes   | 0    | Y-coordinate of the start point.|
| x1   | number | Yes   | 0    | X-coordinate of the end point.|
| y1   | number | Yes   | 0    | Y-coordinate of the end point.|


### createRadialGradient

createRadialGradient(x0: number, y0: number, r0: number, x1: number, y1: number, r1: number): void

Creates a linear gradient.

Since API version 9, this API is supported in ArkTS widgets.

  **Parameters**

| Name  | Type    | Mandatory  | Default Value | Description               |
| ---- | ------ | ---- | ---- | ----------------- |
| x0   | number | Yes   | 0    | X-coordinate of the center of the start circle.        |
| y0   | number | Yes   | 0    | Y-coordinate of the center of the start circle.        |
| r0   | number | Yes   | 0    | Radius of the start circle, which must be a non-negative finite number.|
| x1   | number | Yes   | 0    | X-coordinate of the center of the end circle.        |
| y1   | number | Yes   | 0    | Y-coordinate of the center of the end circle.        |
| r1   | number | Yes   | 0    | Radius of the end circle, which must be a non-negative finite number.|

### createConicGradient<sup>10+</sup>

createConicGradient(startAngle: number, x: number, y: number): CanvasGradient

Creates a conic gradient.

**Parameters**

| Name        | Type    | Mandatory  | Default Value | Description                                 |
| ---------- | ------ | ---- | ---- | ----------------------------------- |
| startAngle | number | Yes   | 0    | Angle at which the gradient starts, in radians. The angle measurement starts horizontally from the right side of the center and moves clockwise.|
| x          | number | Yes   | 0    | X-coordinate of the center of the conic gradient, in vp.                  |
| y          | number | Yes   | 0    | Y-coordinate of the center of the conic gradient, in vp.                  |
