# Enums

>**NOTE**
>
>The initial APIs of this module are supported since API version 7. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Color

Since API version 9, this API is supported in ArkTS widgets.

| Color                    | Value          | Illustration                                    |
| ------------------------ | ------------- | ---------------------------------------- |
| Black                    | 0x000000      | ![en-us_image_0000001219864153](figures/en-us_image_0000001219864153.png) |
| Blue                     | 0x0000ff      | ![en-us_image_0000001174104404](figures/en-us_image_0000001174104404.png) |
| Brown                    | 0xa52a2a      | ![en-us_image_0000001219744201](figures/en-us_image_0000001219744201.png) |
| Gray                     | 0x808080      | ![en-us_image_0000001174264376](figures/en-us_image_0000001174264376.png) |
| Grey                     | 0x808080      | ![en-us_image_0000001174264376](figures/en-us_image_0000001174264376.png) |
| Green                    | 0x008000      | ![en-us_image_0000001174422914](figures/en-us_image_0000001174422914.png) |
| Orange                   | 0xffa500      | ![en-us_image_0000001219662661](figures/en-us_image_0000001219662661.png) |
| Pink                     | 0xffc0cb      | ![en-us_image_0000001219662663](figures/en-us_image_0000001219662663.png) |
| Red                      | 0xff0000      | ![en-us_image_0000001219662665](figures/en-us_image_0000001219662665.png) |
| White                    | 0xffffff      | ![en-us_image_0000001174582866](figures/en-us_image_0000001174582866.png) |
| Yellow                   | 0xffff00      | ![en-us_image_0000001174582864](figures/en-us_image_0000001174582864.png) |
| Transparent<sup>9+</sup> | rgba(0,0,0,0) | Transparent                                     |

## ImageFit

Since API version 9, this API is supported in ArkTS widgets.

| Name       | Description                             |
| --------- | ------------------------------- |
| Contain   | The image is scaled with its aspect ratio retained for the content to be completely displayed within the display boundaries.  |
| Cover     | The image is scaled with its aspect ratio retained for both sides to be greater than or equal to the display boundaries.|
| Auto      | The image is scaled automatically to fit the display area.                          |
| Fill      | The image is scaled to fill the display area, and its aspect ratio is not retained.       |
| ScaleDown | The image is displayed with its aspect ratio retained, in a size smaller than or equal to the original size.            |
| None      | The original size is retained.                      |

## BorderStyle

Since API version 9, this API is supported in ArkTS widgets.

| Name    | Description                           |
| ------ | ----------------------------- |
| Dotted | Dotted border. The radius of a dot is half of **borderWidth**.|
| Dashed | Dashed border.                |
| Solid  | Solid border.                     |

## LineJoinStyle

Since API version 9, this API is supported in ArkTS widgets.

| Name   | Description        |
| ----- | ---------- |
| Bevel | Bevel is used to connect paths.|
| Miter | Miter is used to connect paths.|
| Round | Round is used to connect paths.|

## TouchType

| Name    | Description             |
| ------ | --------------- |
| Down   | A finger is pressed.       |
| Up     | A finger is lifted.       |
| Move   | A finger moves on the screen in pressed state.|
| Cancel | A touch event is canceled.     |

## MouseButton

| Name     | Description      |
| ------- | -------- |
| Left    | Left button on the mouse.   |
| Right   | Right button on the mouse.   |
| Middle  | Middle button on the mouse.   |
| Back    | Back button on the left of the mouse.|
| Forward | Forward button on the left of the mouse.|
| None    | No button.    |

## MouseAction

| Name     | Description     |
| ------- | ------- |
| Press   | The mouse button is pressed.|
| Release | The mouse button is released.|
| Move    | The mouse cursor moves.  |
| Hover   | The mouse pointer is hovered on an element.<br>**NOTE**<br>This value has no effect. |

## Curve

Since API version 9, this API is supported in ArkTS widgets.

| Name                 | Description                                      |
| ------------------- | ---------------------------------------- |
| Linear              | The animation speed keeps unchanged.                       |
| Ease                | The animation starts slowly, accelerates, and then slows down towards the end. The cubic-bezier curve (0.25, 0.1, 0.25, 1.0) is used.|
| EaseIn              | The animation starts at a low speed and then picks up speed until the end. The cubic-bezier curve (0.42, 0.0, 1.0, 1.0) is used.|
| EaseOut             | The animation ends at a low speed. The cubic-bezier curve (0.0, 0.0, 0.58, 1.0) is used.|
| EaseInOut           | The animation starts and ends at a low speed. The cubic-bezier curve (0.42, 0.0, 0.58, 1.0) is used.|
| FastOutSlowIn       | The animation uses the standard cubic-bezier curve (0.4, 0.0, 0.2, 1.0).  |
| LinearOutSlowIn     | The animation uses the deceleration cubic-bezier curve (0.0, 0.0, 0.2, 1.0).  |
| FastOutLinearIn     | The animation uses the acceleration cubic-bezier curve (0.4, 0.0, 1.0, 1.0).  |
| ExtremeDeceleration | The animation uses the extreme deceleration cubic-bezier curve (0.0, 0.0, 0.0, 1.0).  |
| Sharp               | The animation uses the sharp cubic-bezier curve (0.33, 0.0, 0.67, 1.0).|
| Rhythm              | The animation uses the rhythm cubic-bezier curve (0.7, 0.0, 0.2, 1.0).  |
| Smooth              | The animation uses the smooth cubic-bezier curve (0.4, 0.0, 0.4, 1.0).  |
| Friction            | The animation uses the friction cubic-bezier curve (0.2, 0.0, 0.2, 1.0).   |

## AnimationStatus

Since API version 10, this API is supported in ArkTS widgets.

| Name     | Description       |
| ------- | --------- |
| Initial | The animation is in the initial state.  |
| Running | The animation is being played.|
| Paused  | The animation is paused.|
| Stopped | The animation is stopped.|

## FillMode

Since API version 10, this API is supported in ArkTS widgets.

| Name       | Description                                      |
| --------- | ---------------------------------------- |
| None      | Before execution, the animation does not apply any styles to the target component. After execution, the animation restores the target component to its default state.    |
| Forwards  | The target component retains the state set by the last keyframe encountered during execution of the animation.                  |
| Backwards | The animation applies the values defined in the first relevant keyframe once it is applied to the target component, and retains the values during the period set by **delay**. The first relevant keyframe depends on the value of **playMode**. If **playMode** is **Normal** or **Alternate**, the first relevant keyframe is in the **from** state. If **playMode** is **Reverse** or **AlternateReverse**, the first relevant keyframe is in the **to** state.|
| Both      | The animation follows the rules for both **Forwards** and **Backwards**, extending the animation attributes in both directions.|

## PlayMode

Since API version 9, this API is supported in ArkTS widgets.

| Name              | Description                                      |
| ---------------- | ---------------------------------------- |
| Normal           | The animation is played forwards.                                |
| Reverse          | The animation is played backwards.                                 |
| Alternate        | The animation is played forwards for an odd number of times (1, 3, 5...) and backwards for an even number of times (2, 4, 6...).|
| AlternateReverse | The animation is played backwards for an odd number of times (1, 3, 5...) and forwards for an even number of times (2, 4, 6...).|

## KeyType

| Name  | Description   |
| ---- | ----- |
| Down | The key is pressed.|
| Up   | The key is released.|

## KeySource

| Name      | Description        |
| -------- | ---------- |
| Unknown  | Unknown input device. |
| Keyboard | The input device is a keyboard.|

## Edge

| Name                              | Description                                      |
| -------------------------------- | ---------------------------------------- |
| Top                              | Top edge in the vertical direction.<br>Since API version 9, this API is supported in ArkTS widgets.|
| Bottom                           | Bottom edge in the vertical direction.<br>Since API version 9, this API is supported in ArkTS widgets.|
| Start                            | Start position in the horizontal direction.<br>Since API version 9, this API is supported in ArkTS widgets.|
| End                              | End position in the horizontal direction.<br>Since API version 9, this API is supported in ArkTS widgets.|

## Week

| Name  | Description  |
| ---- | ---- |
| Mon  | Monday. |
| Tue  | Tuesday. |
| Wed  | Wednesday. |
| Thur | Thursday. |
| Fri  | Friday. |
| Sat  | Saturday. |
| Sun  | Sunday. |

## Direction

Since API version 9, this API is supported in ArkTS widgets.

| Name  | Description         |
| ---- | ----------- |
| Ltr  | Components are arranged from left to right.  |
| Rtl  | Components are arranged from right to left.  |
| Auto | The default layout direction is used.|

## BarState

Since API version 9, this API is supported in ArkTS widgets.

| Name  | Description                |
| ---- | ------------------ |
| Off  | Not displayed.              |
| On   | Always displayed.             |
| Auto | Displayed when the screen is touched and hidden after 2s.|

## EdgeEffect

Since API version 9, this API is supported in ArkTS widgets.

| Name    | Description                                      |
| ------ | ---------------------------------------- |
| Spring | Spring effect. When at one of the edges, the component can move beyond the bounds through touches, and produces a bounce effect when the user releases their finger.|
| Fade   | Fade effect. When at one of the edges, the component produces a fade effect.                    |
| None   | No effect when the component is at one of the edges.                              |

## Alignment

Since API version 9, this API is supported in ArkTS widgets.

| Name         | Description      |
| ----------- | -------- |
| TopStart    | Top start.  |
| Top         | Horizontally centered on the top. |
| TopEnd      | Top end.   |
| Start       | Vertically centered start.|
| Center      | Horizontally and vertically centered.|
| End         | Vertically centered end. |
| BottomStart | Bottom start.  |
| Bottom      | Horizontally centered on the bottom. |
| BottomEnd   | Bottom end.   |

## TransitionType

Since API version 9, this API is supported in ArkTS widgets.

| Name    | Description                            |
| ------ | ------------------------------ |
| All    | The transition takes effect in all scenarios.|
| Insert | The transition takes effect when a component is inserted or displayed.|
| Delete | The transition takes effect when a component is deleted or hidden.|

## RelateType

| Name  | Description            |
| ---- | -------------- |
| FILL | The current child component is scaled to fill the parent component.|
| FIT  | The current child component is scaled to adapt to the parent component.|

## Visibility

Since API version 9, this API is supported in ArkTS widgets.

| Name     | Description              |
| ------- | ---------------- |
| Hidden  | The component is hidden, and a placeholder is used for it in the layout.   |
| Visible | The component is visible.             |
| None    | The component is hidden. It is not involved in the layout, and no placeholder is used for it.|

## LineCapStyle

Since API version 9, this API is supported in ArkTS widgets.

| Name    | Description                           |
| ------ | ----------------------------- |
| Butt   | The ends of the line are squared off, and the line does not extend beyond its two endpoints.              |
| Round  | The line is extended at the endpoints by a half circle whose diameter is equal to the line width.           |
| Square | The line is extended at the endpoints by a rectangle whose width is equal to half the line width and height equal to the line width.|

## Axis

Since API version 9, this API is supported in ArkTS widgets.

| Name        | Description    |
| ---------- | ------ |
| Vertical   | Vertical direction.|
| Horizontal | Horizontal direction.|

## HorizontalAlign

Since API version 9, this API is supported in ArkTS widgets.

| Name    | Description          |
| ------ | ------------ |
| Start  | Aligned with the start edge in the same direction as the language in use.|
| Center | Aligned with the center. This is the default alignment mode.|
| End    | Aligned with the end edge in the same direction as the language in use. |

## FlexAlign

Since API version 9, this API is supported in ArkTS widgets.

| Name          | Description                                      |
| ------------ | ---------------------------------------- |
| Start        | The child components are aligned with the start edge of the main axis. The first component is aligned with the main-start, and subsequent components are aligned with the previous one.   |
| Center       | The child components are aligned in the center of the main axis. The space between the first component and the main-start is the same as that between the last component and the main-end.  |
| End          | The child components are aligned with the end edge of the main axis. The last component is aligned with the main-end, and other components are aligned with the next one.     |
| SpaceBetween | The child components are evenly distributed along the main axis. The space between any two adjacent components is the same. The first component is aligned with the main-start, the last component is aligned with the main-end, and the remaining components are distributed so that the space between any two adjacent components is the same.|
| SpaceAround  | The child components are evenly distributed along the main axis. The space between any two adjacent components is the same. The space between the first component and main-start, and that between the last component and cross-main are both half the size of the space between two adjacent components.|
| SpaceEvenly  | The child components are evenly distributed along the main axis. The space between the first component and main-start, the space between the last component and main-end, and the space between any two adjacent components are the same.|

## ItemAlign

Since API version 9, this API is supported in ArkTS widgets.

| Name      | Description                                      |
| -------- | ---------------------------------------- |
| Auto     | The default configuration of the flex container is used.                          |
| Start    | The items in the flex container are aligned with the cross-start edge.                   |
| Center   | The items in the flex container are centered along the cross axis.                   |
| End      | The items in the flex container are aligned with the cross-end edge.                   |
| Stretch  | The items in the flex container are stretched and padded along the cross axis. If the flex container has the **Wrap** attribute set to **FlexWrap.Wrap** or **FlexWrap.WrapReverse**, the items are stretched to the cross size of the widest element on the current row or column. In other cases, the items with no size set are stretched to the container size.|
| Baseline | The items in the flex container are aligned in such a manner that their text baselines are aligned along the cross axis.                 |

## FlexDirection

Since API version 9, this API is supported in ArkTS widgets.

| Name           | Description              |
| ------------- | ---------------- |
| Row           | The child components are arranged in the same direction as the main axis runs along the rows. |
| RowReverse    | The child components are arranged opposite to the **Row** direction. |
| Column        | The child components are arranged in the same direction as the main axis runs down the columns. |
| ColumnReverse | The child components are arranged opposite to the **Column** direction.|

## FlexWrap

Since API version 9, this API is supported in ArkTS widgets.

| Name         | Description                         |
| ----------- | --------------------------- |
| NoWrap      | The child components in the flex container are arranged in a single line, and they cannot overflow. |
| Wrap        | The child components in the flex container are arranged in multiple lines, and they may overflow.  |
| WrapReverse | The child components in the flex container are reversely arranged in multiple lines, and they may overflow.|

## VerticalAlign

Since API version 9, this API is supported in ArkTS widgets.

| Name    | Description          |
| ------ | ------------ |
| Top    | Top aligned.       |
| Center | Center aligned. This is the default alignment mode.|
| Bottom | Bottom aligned.       |

## ImageRepeat

Since API version 9, this API is supported in ArkTS widgets.

| Name      | Description           |
| -------- | ------------- |
| X        | The image is repeatedly drawn only along the horizontal axis.|
| Y        | The image is repeatedly drawn only along the vertical axis.|
| XY       | The image is repeatedly drawn along both axes. |
| NoRepeat | The image is not repeatedly drawn.     |

## ImageSize

Since API version 9, this API is supported in ArkTS widgets.

| Type     | Description                                 |
| ------- | ----------------------------------- |
| Cover   | Default value. The image is scaled with its aspect ratio retained for both sides to be greater than or equal to the display boundaries.|
| Contain | The image is scaled with its aspect ratio retained for the content to be completely displayed within the display boundaries.      |
| Auto    | The original image aspect ratio is retained.                         |

## GradientDirection

Since API version 9, this API is supported in ArkTS widgets.

| Name         | Description   |
| ----------- | ----- |
| Left        | The gradient direction is from right to left.|
| Top         | The gradient direction is from bottom to top.|
| Right       | The gradient direction is from left to right.|
| Bottom      | The gradient direction is from top to bottom.|
| LeftTop     | The gradient direction is upper left.  |
| LeftBottom  | The gradient direction is lower left.  |
| RightTop    | The gradient direction is upper right.  |
| RightBottom | The gradient direction is lower right.  |
| None        | No gradient.   |

## SharedTransitionEffectType

| Name      | Description                                      |
| -------- | ---------------------------------------- |
| Static   | The element position remains unchanged on the target page, and transition opacity can be configured. Currently, this effect is only valid in redirecting to the target page.|
| Exchange | The element is relocated and scaled properly on the target page.                 |

## FontStyle

Since API version 9, this API is supported in ArkTS widgets.

| Name    | Description      |
| ------ | -------- |
| Normal | Standard font style.|
| Italic | Italic font style.|

## FontWeight

Since API version 9, this API is supported in ArkTS widgets.

| Name     | Description     |
| ------- | ------- |
| Lighter | The font weight is lighter.  |
| Normal  | The font weight is normal.|
| Regular | The font weight is regular.|
| Medium  | The font weight is medium.|
| Bold    | The font weight is bold.  |
| Bolder  | The font weight is bolder. |

## TextAlign

Since API version 9, this API is supported in ArkTS widgets.

| Name                   | Description     |
| --------------------- | ------- |
| Start                 | Aligned with the start.|
| Center                | Horizontally centered.|
| End                   | Aligned with the end.|
| JUSTIFY<sup>10+</sup> | Aligned with both margins.  |

## TextOverflow

Since API version 9, this API is supported in ArkTS widgets.

| Name                   | Description                 |
| --------------------- | ------------------- |
| None                  | Extra-long text is clipped.         |
| Clip                  | Extra-long text is clipped.       |
| Ellipsis              | An ellipsis (...) is used to represent text overflow.|
| MARQUEE<sup>10+</sup> | Text continuously scrolls when text overflow occurs.    |

## TextDecorationType

Since API version 9, this API is supported in ArkTS widgets.

| Name         | Description       |
| ----------- | --------- |
| Underline   | Line under the text. |
| LineThrough | Line through the text.|
| Overline    | Line over the text. |
| None        | No decorative lines.|

## TextCase

Since API version 9, this API is supported in ArkTS widgets.

| Name       | Description        |
| --------- | ---------- |
| Normal    | The original case of the text is retained.|
| LowerCase | All letters in the text are in lowercase.  |
| UpperCase | All letters in the text are in uppercase.  |

## ResponseType<sup>8+</sup>

| Name        | Description           |
| ---------- | ------------- |
| LongPress  | The menu is displayed when the component is long-pressed.  |
| RightClick | The menu is displayed when the component is right-clicked.|

## HoverEffect<sup>8+</sup>

| Name       | Description            |
| --------- | -------------- |
| Auto      | Default hover effect.|
| Scale     | Scale effect.       |
| Highlight | Background fade-in and fade-out effect.  |
| None      | No effect.        |

## Placement<sup>8+</sup>

| Name           | Description                                    |
| ------------- | -------------------------------------- |
| Left          | The popup is on the left of the component, vertically aligned with the component on the left.                 |
| Right         | The popup is on the right of the component, vertically aligned with the component on the right.                 |
| Top           | The popup is at the top of the component, horizontally aligned with the component at the top.                 |
| Bottom        | The popup is at the bottom of the component, horizontally aligned with the component at the bottom.                 |
| TopLeft       | The popup is at the top of the component and, since API version 9, aligned with the left of the component.|
| TopRight      | The popup is at the top of the component and, since API version 9, aligned with the right of the component.|
| BottomLeft    | The popup is at the bottom of the component and, since API version 9, aligned with the left of the component.|
| BottomRight   | The popup is at the bottom of the component and, since API version 9, aligned with the right of the component.|
| LeftTop9+     | The popup is on the left of the component and aligned with the top of the component.                 |
| LeftBottom9+  | The popup is on the left of the component and aligned with the bottom of the component.                 |
| RightTop9+    | The popup is on the right of the component and aligned with the top of the component.                 |
| RightBottom9+ | The popup is on the right of the component and aligned with the bottom of the component.                 |

## CopyOptions<sup>9+</sup>

Since API version 9, this API is supported in ArkTS widgets.

| Name         | Description      |
| ----------- | -------- |
| None        | Copy is not allowed.  |
| InApp       | Intra-application copy is allowed.|
| LocalDevice | Intra-device copy is allowed.|

## HitTestMode<sup>9+</sup>

| Name         | Description                                      |
| ----------- | ---------------------------------------- |
| Default     | Both the node and its child node respond to the hit test of a touch event, but its sibling node is blocked from the hit test.|
| Block       | The node responds to the hit test of a touch event, but its child node and sibling node are blocked from the hit test.|
| Transparent | Both the node and its child node respond to the hit test of a touch event, and its sibling node is also considered during the hit test.|
| None        | The node does not respond to the hit test of a touch event, but its child node and sibling node are considered during the hit test.     |

## BlurStyle<sup>9+</sup>

This API is supported in ArkTS widgets.

| Name                  | Description       |
| -------------------- | --------- |
| Thin                 | Thin material.  |
| Regular              | Regular material.|
| Thick                | Thick material.   |
| BACKGROUND_THIN       | Material that creates the minimum depth of field effect.  |
| BACKGROUND_REGULAR    | Material that creates a medium shallow depth of field effect.  |
| BACKGROUND_THICK      | Material that creates a high shallow depth of field effect.  |
| BACKGROUND_ULTRA_THICK | Material that creates the maximum depth of field effect. |
| NONE<sup>10+</sup> | No blur. |

## ThemeColorMode<sup>10+</sup>

| Name    | Description        |
| ------ | ---------- |
| SYSTEM | Following the system color mode.|
| LIGHT  | Light color mode. |
| DARK   | Dark color mode. |

## AdaptiveColor<sup>10+</sup>

| Name     | Description                       |
| ------- | ------------------------- |
| DEFAULT | Adaptive color mode is not used. The default color is used as the mask color.   |
| AVERAGE | Adaptive color mode is used. The average color value of the color picking area is used as the mask color.|

## TextHeightAdaptivePolicy<sup>10+</sup>

| Name                     | Description                      |
| ----------------------- | ------------------------ |
| MAX_LINES_FIRST         | Prioritize the **maxLines** settings.|
| MIN_FONT_SIZE_FIRST     | Prioritize the **minFontSize** settings.    |
| LAYOUT_CONSTRAINT_FIRST | Prioritize the layout constraint settings in terms of height.|

## ObscuredReasons<sup>10+</sup>

This API is supported in ArkTS widgets.

| Name       | Description                    |
| ----------- | ------------------------ |
| PLACEHOLDER | The content is replaced by a placeholder.|

## TransitionEdge<sup>10+<sup>

| Name    | Description    |
| ------ | ------ |
| TOP    | Top edge of the window.|
| BOTTOM | Bottom edge of the window.|
| START  | Left edge of the window.|
| END    | Right edge of the window.|

## ClickEffectLevel<sup>10+<sup>

| Name  | Description              | Animation Settings                         | Default Zoom Ratio                    |
| ------ | --------------------------------- | --------------------------------- | --------------------------------- |
| LIGHT  | Small area (light)| Spring effect, with stiffness of 410, damping of 38, and initial velocity of 1.| 90% |
| MIDDLE | Medium area (stable)| Spring effect, with stiffness of 350, damping of 35, and initial velocity of 0.5.| 95% |
| HEAVY  | Large area (heavy)| Spring effect, with stiffness of 240, damping of 28, and initial velocity of 0.| 95% |

## TextContentStyle<sup>10+</sup>

| Name   | Description                                                        |
| ------- | ------------------------------------------------------------ |
| DEFAULT | Default style. The caret width is fixed at 1.5 vp, and the caret height is subject to the background height and font size of the selected text.|
| INLINE  | Inline input style. The background height of the selected text is the same as the height of the text box.<br>The **showError** attribute is not supported for this style.|

## MenuPreviewMode <sup>11+</sup>

| Name | Description                                  |
| ----- | -------------------------------------- |
| NONE  | No preview is displayed.                      |
| IMAGE | The preview is a screenshot of the component on which a long-press triggers the context menu.|

## ImageSmoothingQuality<sup>8+</sup>

Since API version 9, this API is supported in ArkTS widgets.

| Name       | Description            |
| -------- | -------------- |
| low      | Low quality.|
| medium   | Medium quality.|
| high     | High quality.|

## CanvasDirection<sup>8+</sup>

Since API version 9, this API is supported in ArkTS widgets.

| Name       | Description            |
| -------- | -------------- |
| inherit | The text direction is inherited from the **\<Canvas>** component.|
| ltr     | The text direction is from left to right.|
| rtl     | The text direction is from right to left.|
