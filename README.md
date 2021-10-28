## What CSS is?

- CSS stands for **Cascading Style Sheets**
- CSS (Cascading Style Sheets) is a declarative language that controls how webpages look in the browser.
- The browser applies CSS style declarations to selected elements to display them properly.
- A style declaration contains the properties and their values, which determine how a webpage looks.
- CSS is one of the three core Web technologies, along with HTML and JavaScript.
- CSS usually styles HTML elements, but can be also used with other markup languages like SVG or XML.

## Basics

- **Both properties and values are case-insensitive** by default in CSS.
- **The pair is separated by a colon**, ':' (U+003A COLON), **and white spaces** before, between, and after properties and values, but not necessarily inside, **are ignored**.
- **Declarations are grouped in blocks, that is in a structure delimited by an opening brace,** '{' (U+007B LEFT CURLY BRACKET), **and a closing one**, '}' (U+007D RIGHT CURLY BRACKET).
- Such blocks are naturally called declaration blocks and **declarations** inside them **are separated by a semi-colon,** ';' (U+003B SEMICOLON).
- **Each (valid) declaration block is preceded by one or more comma-separated selectors**, which are conditions selecting some elements of the page. **A selector group and an associated declarations block, together, are called a ruleset, or often a rule**.
- **As an element of the page may be matched by several selectors**, and therefore by several rules potentially containing a given property several times, with different values, **the CSS standard defines which one has precedence over the other and must be applied: this is called the cascade algorithm.**
- **If one single basic selector is invalid**, like when using an unknown pseudo-element or pseudo-class, **the whole selector is invalid and therefore the entire rule is ignored** (as invalid too).
- **A statement is a building block that begins with any non-space characters and ends at the first closing brace or semi-colon** (outside a string, non-escaped and not included into another {}, () or [] pair).

  ![statements diagram](https://developer.mozilla.org/en-US/docs/Web/CSS/Syntax/css_syntax_-_statements_venn_diag.png)
  
- **At-rules that start with an at sign, '@'** (U+0040 COMMERCIAL AT), **followed by an identifier and then continuing up to the end of the statement, that is up to the next semi-colon (;)** outside of a block, or the end of the next block. **Each type of at-rules, defined by the identifier, may have its own internal syntax, and semantics** of course. They are used to convey meta-data information (like @charset or @import), conditional information (like @media or @document), or descriptive information (like @font-face).

### Cascade

- **Stylesheets cascade** — at a very simple level, **this means that the order of CSS rules matter; when two rules apply that have equal specificity the one that comes last in the CSS is the one that will be used**.

### Specificity

- **Specificity is how the browser decides which rule applies if multiple rules have different selectors**, but could still apply to the same element. **It is basically a measure of how specific a selector's selection will be**.
- There are three factors to consider, listed here in increasing order of importance:
  - **Source order: If you have more than one rule, which has exactly the same weight, then the one that comes last in the CSS will win**. You can think of this as rules which are nearer the element itself overwriting early ones until the last one wins and gets to style the element.
  - **Specificity:** The amount of specificity a selector has is measured using four different values (or components), which can be thought of as thousands, hundreds, tens, and ones — four single digits in four columns:
    1. **Thousands**: Score one in this column if the declaration is inside a style attribute, aka inline styles. Such declarations don't have selectors, so their specificity is always 1000.
    2. **Hundreds**: Score one in this column for each ID selector contained inside the overall selector.
    3. **Tens**: Score one in this column for each class selector, attribute selector, or pseudo-class contained inside the overall selector.
    4. **Ones**: Score one in this column for each element selector or pseudo-element contained inside the overall selector.

    Selector | Thousands | Hundreds | Tens | Ones | Total specificity
    -------- | --------- | -------- | ---- | ---- | -----------------
    `h1` | 0 | 0 | 0 | 1 | 0001
    `h1 + p::first-letter` | 0 | 0 | 0 | 3 | 0003
    `li > a[href*="en-US"] > .inline-warning` | 0 | 0 | 2 | 2 | 0022
    `#identifier` | 0 | 1 | 0 | 0 | 0100
    No selector, with a rule inside an element's style attribute | 1 | 0 | 0 | 0 | 1000
  - The universal selector (*), combinators (+, >, ~, ' '), and negation pseudo-class (:not) **have no effect on specificity.**

- Some examples:
```css
/* specificity: 0101 */
#outer a {
    background-color: red;
}
        
/* specificity: 0201 */
#outer #inner a {
    background-color: blue;
}

/* specificity: 0104 */
#outer div ul li a {
    color: yellow;
}

/* specificity: 0113 */
#outer div ul .nav a {
    color: white;
}

/* specificity: 0024 */
div div li:nth-child(2) a:hover {
    border: 10px solid black;
}

/* specificity: 0023 */
div li:nth-child(2) a:hover {
    border: 10px dashed black;
}

/* specificity: 0033 */
div div .nav:nth-child(2) a:hover {
    border: 10px double black;
}
```
- *The first two selectors are competing over the styling of the link's background color — the second one wins and
makes the background color blue because it has an extra ID selector in the chain: its specificity is 201 vs. 101.*
- *The third and fourth selectors are competing over the styling of the link's text color — the second one wins and
makes the text white because although it has one less element selector, the missing selector is swapped out for a
class selector, which is worth ten rather than one. So the winning specificity is 113 vs. 104.*
- *Selectors 5–7 are competing over the styling of the link's border when hovered. Selector six clearly loses to
five with a specificity of 23 vs. 24 — it has one fewer element selectors in the chain. Selector seven, however,
beats both five and six — it has the same number of sub-selectors in the chain as five, but an element has been
swapped out for a class selector. So the winning specificity is 33 vs. 23 and 24.*

### Importance

- **!important** - is used to make a particular property and value the most specific thing, thus overriding the normal rules of the cascade.
- The only way to override this !important declaration would be to include another !important declaration on a declaration with the same specificity later in the source order, or one with higher specificity.
- **However, it's strongly recommended that !important should never be used unless you absolutely have to.**

### Inheritance

- **Some CSS property values set on parent elements are inherited by their child elements, and some aren't**.
- Things like widths (as mentioned above), margins, padding, and borders do not inherit.
- **CSS provides four special universal property values for controlling inheritance**. Every CSS property accepts these values.
  - **inherit:** Sets the property value applied to a selected element to be the same as that of its parent element. Effectively, this "turns on inheritance".
  - **initial:** Sets the property value applied to a selected element to the initial value of that property.
  - **unset:** Resets the property to its natural value, which means that if the property is naturally inherited it acts like inherit, otherwise it acts like initial.
  - **revert:** Acts like unset in many cases, however will revert the property to the browser's default styling rather than the defaults applied to that property.
- **The CSS shorthand property all can be used to apply one of these inheritance values to (almost) all properties at once**. Its value can be any one of the inheritance values (inherit, initial, unsetor revert).

## Selectors

- The element or elements which are selected by the selector are referred to as the subject of the **selector**.
- Individual **selectors can be combined** into a selector list so that the rule is applied to all of the individual selectors.
- When you group selectors, if any selector is invalid the whole rule will be ignored.

### Type of selectors

- type selector
  - A type selector is sometimes referred to as a tag name selector or element selector because it selects an HTML tag/element in your document.
  ```css
  h1 { ... }
  ```
- class selector
  - It will select everything in the document with that class applied to it.
  ```css
  .box { ... }
  ```
  - You can create a selector that will target specific elements with the class applied, by using the type selector for the element we want to target, with the class appended using a dot, with no white space in between.
  ```css
  span.box { ... }
  ```
- id selector
  - An ID selector begins with a # rather than a dot character, but is used in the same way as a class selector.
  - However, an ID can be used only once per page, and elements can only have a single id value applied to them.
  ```css
  #unique { ... }
  ```
- universal selector
  - The universal selector is indicated by an asterisk (*). **It selects everything in the document (or inside the parent element** if it is being chained together with another element and a descendant combinator). 
  ```css
  * { ... }
  ```
- attribute selector
  - In CSS you can use attribute selectors to target elements with certain attributes.
  - These selectors enable the selection of an element based on the presence of an attribute alone, or on various different matches against the value of the attribute.

  Selector | Example | Description
  -------- | ------- | -----------
  `[attr]` | `a[title]` | Matches elements with an attr attribute (whose name is the value in square brackets).
  `[attr=value]` | `a[href="https://example.com"]` | Matches elements with an attr attribute whose value is exactly value — the string inside the quotes.
  `[attr~=value]` | `p[class~="special"]` | Matches elements with an attr attribute whose value is exactly value, or contains value in its (space separated) list of values.
  [attr&#124;=value] | div[lang&#124;="zh"] | Matches elements with an attr attribute whose value is exactly value or begins with value immediately followed by a hyphen.
  
- substring matching selectors (part of attr selectors)
  - These selectors allow for more advanced matching of substrings inside the value of your attribute.

  Selector | Example | Description
  -------- | ------- | -----------
  `[attr^=value]` | `li[class^="box-"]` | Matches elements with an attr attribute, whose value begins with value.
  `[attr$=value]` | `li[class$="-box"]` | Matches elements with an attr attribute whose value ends with value.
  `[attr*=value]` | `li[class*="box"]` | Matches elements with an attr attribute whose value contains value anywhere within the string.
  - If you want to **match attribute values case-insensitively you can use the value i before the closing bracket**. This flag tells the browser to match ASCII characters case-insensitively. Without the flag the values will be matched according to the case-sensitivity of the document language — in HTML's case it will be case sensitive.
  ```css
  li[class^="a" i] {
    color: red;
  }
  ```
  - There is also a newer value **s**, which will **force case-sensitive matching in contexts** where matching is normally case-insensitive, **however this is less well supported in browsers** and isn't very useful in an HTML context.
- pseudo-classes
  - **A pseudo-class is a selector that selects elements that are in a specific state**, e.g. they are the first element of their type, or they are being hovered over by the mouse pointer. They tend to act as if you had applied a class to some part of your document, often helping you cut down on excess classes in your markup, and giving you more flexible, maintainable code.
  - Pseudo-classes are keywords that **start with a colon**.
  - It is valid to write pseudo-classes and elements without any element selector preceding them. However, usually you want more control than that, so you need to be more specific.
  - **Some pseudo-classes only apply when the user interacts with the document in some way**. These user-action pseudo-classes, sometimes referred to as **dynamic pseudo-classes**, act as if a class had been added to the element when the user interacts with it.
  ```css
  /* this only applies if the user moves their pointer over an element, typically a link. */
  a:hover { ... }
  ```
  
    Selector | Description
    -------- | -----------
    `:active` | Matches when the user activates (for example clicks on) an element.
    `:any-link` | Matches both the :link and :visited states of a link.
    `:blank` | Matches an `<input>` element whose input value is empty.
    `:checked` | Matches a radio button or checkbox in the selected state.
    `:current` | Matches the element, or an ancestor of the element, that is currently being displayed.
    `:default` | Matches the one or more UI elements that are the default among a set of similar elements.
    `:dir` | Select an element based on its directionality (value of the HTML dir attribute or CSS direction property).
    `:disabled` | Matches user interface elements that are in an disabled state.
    `:empty` | Matches an element that has no children except optionally white space.
    `:enabled` | Matches user interface elements that are in an enabled state.
    `:first` | In Paged Media, matches the first page.
    `:first-child` | Matches an element that is first among its siblings.
    `:first-of-type` | Matches an element which is first of a certain type among its siblings.
    `:focus` | Matches when an element has focus.
    `:focus-visible` | Matches when an element has focus and the focus should be visible to the user.
    `:focus-within` | Matches an element with focus plus an element with a descendent that has focus.
    `:future` | Matches the elements after the current element.
    `:hover` | Matches when the user hovers over an element.
    `:indeterminate` | Matches UI elements whose value is in an indeterminate state, usually checkboxes.
    `:in-range` | Matches an element with a range when its value is in-range.
    `:invalid` | Matches an element, such as an `<input>`, in an invalid state.
    `:lang` | Matches an element based on language (value of the HTML lang attribute).
    `:last-child` | Matches an element which is last among its siblings.
    `:last-of-type` | Matches an element of a certain type that is last among its siblings.
    `:left` | In Paged Media, matches left-hand pages.
    `:link` | Matches unvisited links.
    `:local-link` | Matches links pointing to pages that are in the same site as the current document.
    `:is()` | Matches any of the selectors in the selector list that is passed in.
    `:not` | Matches things not matched by selectors that are passed in as a value to this selector.
    `:nth-child` | Matches elements from a list of siblings — the siblings are matched by a formula of the form an+b (e.g. 2n + 1 would match elements 1, 3, 5, 7, etc. All the odd ones.)
    `:nth-of-type` | Matches elements from a list of siblings that are of a certain type (e.g. <p> elements) — the siblings are matched by a formula of the form an+b (e.g. 2n + 1 would match that type of element, numbers 1, 3, 5, 7, etc. All the odd ones.)
    `:nth-last-child` | Matches elements from a list of siblings, counting backwards from the end. The siblings are matched by a formula of the form an+b (e.g. 2n + 1 would match the last element in the sequence, then two elements before that, then two elements before that, etc. All the odd ones, counting from the end.)
    `:nth-last-of-type` | Matches elements from a list of siblings that are of a certain type (e.g. <p> elements), counting backwards from the end. The siblings are matched by a formula of the form an+b (e.g. 2n + 1 would match the last element of that type in the sequence, then two elements before that, then two elements before that, etc. All the odd ones, counting from the end.)
    `:only-child` | Matches an element that has no siblings.
    `:only-of-type` | Matches an element that is the only one of its type among its siblings.
    `:optional` | Matches form elements that are not required.
    `:out-of-range` | Matches an element with a range when its value is out of range.
    `:past` | Matches the elements before the current element.
    `:placeholder-shown` | Matches an input element that is showing placeholder text.
    `:playing` | Matches an element representing an audio, video, or similar resource that is capable of being “played” or “paused”, when that element is “playing”.
    `:paused` | Matches an element representing an audio, video, or similar resource that is capable of being “played” or “paused”, when that element is “paused”.
    `:read-only` | Matches an element if it is not user-alterable.
    `:read-write` | Matches an element if it is user-alterable.
    `:required` | Matches form elements that are required.
    `:right` | In Paged Media, matches right-hand pages.
    `:root` | Matches an element that is the root of the document.
    `:scope` | Matches any element that is a scope element.
    `:valid` | Matches an element such as an `<input>` element, in a valid state.
    `:target` | Matches an element if it is the target of the current URL (i.e. if it has an ID matching the current URL fragment).
    `:visited` | Matches visited links.

- pseudo-elements
  - Pseudo-elements act as if you had added a whole new HTML element into the markup, rather than applying a class to existing elements.
  - Pseudo-elements start with a **double colon**.
  - Modern browsers support the early pseudo-elements with single- or double-colon syntax for backwards compatibility.
  ```css
  p::first-line { ... }
  ```
  - **Generating content with ::before and ::after**
    - There are a couple of special pseudo-elements, which are used along with the content property to insert content into your document using CSS.
    - You could use these to insert a string of text.
    - A more valid use of these pseudo-elements is to insert an icon.
    ```css
    .box::after {
      content: " >"
    }   
    ```
    - These pseudo-elements are also frequently used to insert an empty string, which can then be styled just like any element on the page.
    ```css
    p::before {
      content: "";
      display: block;
      width: 100px;
      height: 100px;
      background-color: rebeccapurple;
      border: 1px solid black;
    } 
    ```
    - The use of the ::before and ::after pseudo-elements along with the content property is referred to as "Generated Content" in CSS

    Selector | Description
    -------- | -----------
    `::after` | Inserts a stylable element appearing after the originating element's actual content, if used with the content property with a value other than none.
    `::before` | Inserts a stylable element appearing before the originating element's actual content, if used with the content property with a value other than none.
    `::first-letter` | Matches the first letter of the element.
    `::first-line` | Matches the first line of the containing element.
    `::grammar-error` | Matches a portion of the document containing a grammar error as flagged by the browser.
    `::marker` | Matches the marker box of a list item, which typically contains a bullet or number.
    `::selection` | Matches the portion of the document that has been selected.
    `::spelling-error` | Matches a portion of the document containing a spelling error as flagged by the browser.
    
- descendant combinator
    - The descendant combinator — typically represented by a single space ( ) character — **combines two selectors such that elements matched by the second selector are selected if they have an ancestor** (parent, parent's parent, parent's parent's parent, etc) **element matching the first selector.**
  ```css
  article p { ... }
  ```
- child combinator
    - The child combinator (>) is placed between two CSS selectors.
    - **It matches only those elements matched by the second selector that are the direct children of elements matched by the first.**
    - Descendent elements further down the hierarchy don't match.
  ```css
  article > p { ... }
  ```
- adjacent sibling combinator
    - The adjacent sibling selector (+) is placed between two CSS selectors.
    - **It matches only those elements matched by the second selector that are the next sibling element of the first selector.**
    - A common use case is to do something with a paragraph that follows a heading.
  ```css
  h1 + p { ... }
  ```
- general sibling combinator
    - If you want to select siblings of an element even if they are not directly adjacent, then you can use the general sibling combinator (~).
  ```css
  h1 ~ p { ... }
  ```

## The box model

- Everything in CSS has a box around it.
- In CSS we broadly have two types of boxes — **block boxes** and **inline boxes**. These characteristics refer to how the box behaves in terms of page flow and in relation to other boxes on the page.
- Boxes also have an **inner display** type and an **outer display** type.
  - **Outer display types:**
    - **It details whether the box is block or inline.**
    - **If a box has an outer display type of block, it will behave in the following ways:**
      - The box will break onto a new line.
      - The box will extend in the inline direction to fill the space available in its container. In most cases this means that the box will become as wide as its container, filling up 100% of the space available.
      - The width and height properties are respected.
      - Padding, margin and border will cause other elements to be pushed away from the box
      - *Some HTML elements, such as `<h1>` and `<p>`, use block as their outer display type by default.*
    - **If a box has an outer display type of inline, then:**
      - The box will not break onto a new line.
      - The width and height properties will not apply.
      - Vertical padding, margins, and borders will apply but will not cause other inline boxes to move away from the box.
      - Horizontal padding, margins, and borders will apply and will cause other inline boxes to move away from the box.
      - Some HTML elements, such as `<a>`, `<span>`, `<em>` and `<strong>` use inline as their outer display type by default.
  - **Inner display types:**
    - It dictates how elements inside that box are laid out.
    - By default, the elements inside a box are laid out in normal flow, which means that they behave just like any other block and inline elements
  - **display: inline-block:**
    - A special value of display, which provides a middle ground between inline and block.
    - This is useful for situations where you do not want an item to break onto a new line, but do want it to respect width and height and avoid the overlapping seen above.
    - *This can be useful when you want to give a link a larger hit area by adding padding. `<a>` is an inline element like <span>; you can use display: inline-block to allow padding to be set on it, making it easier for a user to click the link.*
- **The CSS box model as a whole applies to block boxes. Inline boxes use just some of the behavior defined in the box model.** The model defines how the different parts of a box — margin, border, padding, and content — work together to create a box that you can see on a page.
- Parts of a box:

  ![parts of a box](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/The_box_model/box-model.png)
  
  - **Content box:**
    - The area where your content is displayed, which can be sized using properties like width and height.
  - **Padding box:**
    - The padding sits around the content as white space; its size can be controlled using padding and related properties.
    - You **cannot have negative amounts of padding, so the value must be 0 or a positive value**.
    - Padding is typically used to push the content away from the border.
    - **Any background applied to your element will display behind the padding.**
  - **Border box:**
    - The border box wraps the content and any padding.
    - Its size and style can be controlled using border and related properties.
    - If you are using the standard box model, the size of the border is added to the width and height of the box.
    - If you are using the alternative box model then the size of the border makes the content box smaller as it takes up some of that available width and height.
  - **Margin box:**
    - The margin is the outermost layer, wrapping the content, padding, and border as whitespace between this box and other elements.
    - The margin is an invisible space around your box. It pushes other elements away from the box.
    - Its size can be controlled using margin and related properties.
    - Margins can have positive or negative values.
    - Setting a negative margin on one side of your box can cause it to overlap other things on the page.
    - Whether you are using the standard or alternative box model, the margin is always added after the size of the visible box has been calculated.
    - **Margin collapsing:**
      - A key thing to understand about margins is the concept of margin collapsing.
      - **If** you have two elements whose margins touch, and **both margins are positive, those margins will combine** to become one margin, **and its size will be equal to the largest individual margin.**
      - **If one margin is negative, its value will be subtracted from the total.**
      - **Where both are negative, the margins will collapse and the largest value will be used.**
      - *The margins of floating and absolutely positioned elements never collapse.*
      - The **margins of adjacent siblings are collapsed** (except when the latter sibling needs to be cleared past floats).
      - **Empty boxes:** If there is no border, padding, inline content, height, or min-height to separate a block's margin-top from its margin-bottom, then its top and bottom margins collapse.
      - **If there is no border, padding, inline part, block formatting context created, or clearance to separate** the margin-top of **a block from** the margin-top of **one or more of its descendant blocks**; or no border, padding, inline content, height, or min-height to separate the margin-bottom of a block from the margin-bottom of one or more of its descendant blocks, **then those margins collapse. The collapsed margin ends up outside the parent.**
      - These rules apply even to margins that are zero, so the margin of a descendant ends up outside its parent (according to the rules above) whether or not the parent's margin is zero.
- **The margin is not counted towards the actual size of the box** — sure, it affects the total space that the box will take up on the page, but only the space outside the box. **The box's area stops at the border** — it does not extend into the margin.
### The standard CSS box model

- In the **standard box model**, if you give a box a width and a height attribute, this defines the width and height of the content box. Any padding and border is then added to that width and height to get the total size taken up by the box.
  - Example:
    ```css
      .box {
        width: 350px;
        height: 150px;
        margin: 10px;
        padding: 25px;
        border: 5px solid black;
      }
    ```
    - *The actual space taken up by the box will be 410px wide (350 + 25 + 25 + 5 + 5) and 210px high (150 + 25 + 25 + 5 + 5).*
  
  ![standard box model](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/The_box_model/standard-box-model.png)

### The alternative CSS box model

- **By default, browsers use the standard box model**. If you want to **turn on the alternative model** for an element, you do so **by setting** `box-sizing: border-box` on it.
- Using this model, **any width is the width of the visible box on the page**, therefore the content area width is that width minus the width for the padding and border. The same CSS as used above would give the below result (width = 350px, height = 150px).

  ![alternate box model](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/The_box_model/alternate-box-model.png)

## Styling backgrounds in CSS

- The CSS background property is a shorthand for a number of background longhand properties.

### Background colors

- The background-color property defines the background color on any element in CSS.
- The property accepts any valid `<color>`.
- **A background-color extends underneath the content and padding box of the element.**
- **When dealing with multiple background images, a background-color may only be specified after the final comma.**

### Background images

- The `background-image` property enables the display of an image in the background of an element.
- **By default, the large image is not scaled down** to fit the box, so we only see a small corner of it, **whereas the small image is tiled to fill the box**.
- **If you specify a background color in addition to a background image then the image displays on top of the color.**
- **If a specified image cannot be drawn** (for example, when the file denoted by the specified URI cannot be loaded), **browsers handle it as they would a none value.**
- Even if the images are opaque and the color won't be displayed in normal circumstances, **web developers should always specify a background-color**. If the images cannot be loaded—for instance, when the network is down—the background color will be used as a fallback.
- **To specify multiple background images, supply multiple values, separated by a comma**.

#### Controlling background-repeat

- The background-repeat property is used to control the tiling behavior of images.
- The available values are:
  - **repeat** — **The default**. The image is repeated as much as needed to cover the whole background image painting area. The last image will be clipped if it doesn't fit.
  - **no-repeat** — The image is not repeated (and hence the background image painting area will not necessarily be entirely covered). The position of the non-repeated background image is defined by the background-position CSS property.
  - **repeat-x** — repeat horizontally.
  - **repeat-y** — repeat vertically.
  - **space** — **The image is repeated as much as possible without clipping. The first and last images are pinned to either side of the element, and whitespace is distributed evenly between the images.** The background-position property is ignored unless only one image can be displayed without clipping. The only case where clipping happens using space is when there isn't enough room to display one image.
  - **round** — As the allowed space increases in size, the repeated images will stretch (leaving no gaps) until there is room (space left >= half of the image width) for another one to be added. When the next image is added, all of the current ones compress to allow room.
  ```css
  /* Keyword values */
  background-repeat: repeat-x;
  background-repeat: repeat-y;
  background-repeat: repeat;
  background-repeat: space;
  background-repeat: round;
  background-repeat: no-repeat;
  
  /* Two-value syntax: horizontal | vertical */
  background-repeat: repeat space;
  background-repeat: repeat repeat;
  background-repeat: round space;
  background-repeat: no-repeat round;
  
  /* Global values */
  background-repeat: inherit;
  background-repeat: initial;
  background-repeat: revert;
  background-repeat: unset;
  ```

#### Sizing the background image

- The background-size property is specified in one of the following ways:
  - Using the keyword values `contain` or `cover`.
  - Using a width value only, in which case the height defaults to auto.
  - Using both a width and a height value, in which case the first sets the width and the second sets the height. Each value can be a `<length>`, a `<percentage>`, or auto.
- **To specify the size of multiple background images, separate the value for each one with a comma.**
- When dealing with multiple backgrounds The value of background-size may only be included immediately after background-position, separated with the `/` character, like this: `center/80%`.
- Available keywords:
  - `cover` — the browser will make the image just large enough so that it completely covers the box area while still retaining its aspect ratio. In this case, part of the image is likely to end up outside the box.
  - `contain` — the browser will make the image the right size to fit inside the box. In this case, you may end up with gaps on either side or on the top and bottom of the image, if the aspect ratio of the image is different from that of the box.
  - `auto` — Scales the background image in the corresponding direction such that its intrinsic proportions are maintained.
- Spaces not covered by a background image are filled with the background-color property, and the background color will be visible behind background images that have transparency/translucency.
- **Intrinsic dimensions and proportions:**
  - The computation of values depends on the image's intrinsic dimensions (**width and height**) and intrinsic proportions (**width-to-height ratio**):
    - A **bitmap image** (such as JPG) **always has intrinsic dimensions and proportions**.
    - A **vector image** (such as SVG) **does not necessarily have intrinsic dimensions**. If it has both horizontal and vertical intrinsic dimensions, it also has intrinsic proportions. If it has no dimensions or only one dimension, it may or may not have proportions.
    - **CSS `<gradient>` have no intrinsic dimensions or intrinsic proportions.**
    - *Background images created with the element() function use the intrinsic dimensions and proportions of the generating element.*
  - Based on the intrinsic dimensions and proportions, the rendered size of the background image is computed as follows:
    - **If both components of background-size are specified and are not auto**: The background image is rendered at the specified size.
    - If the background-size is contain or cover: While preserving its intrinsic proportions, the image is rendered at the largest size contained within, or covering, the background positioning area. If the image has no intrinsic proportions, then it's rendered at the size of the background positioning area.
    - **If the background-size is auto or auto auto:**
      - If the image has both horizontal and vertical intrinsic dimensions, it's rendered at that size.
      - If the image has no intrinsic dimensions and has no intrinsic proportions, it's rendered at the size of the background positioning area.
      - If the image has no intrinsic dimensions but has intrinsic proportions, it's rendered as if contain had been specified instead.
      - If the image has only one intrinsic dimension and has intrinsic proportions, it's rendered at the size corresponding to that one dimension. The other dimension is computed using the specified dimension and the intrinsic proportions.
      - If the image has only one intrinsic dimension but has no intrinsic proportions, it's rendered using the specified dimension and the other dimension of the background positioning area.
      - **SVG images have a preserveAspectRatio attribute that defaults to the equivalent of contain; an explicit background-size causes preserveAspectRatio to be ignored.**
    - If the background-size has one auto component and one non-auto component:
      - If the image has intrinsic proportions, it's stretched to the specified dimension. The unspecified dimension is computed using the specified dimension and the intrinsic proportions.
      - If the image has no intrinsic proportions, it's stretched to the specified dimension. The unspecified dimension is computed using the image's corresponding intrinsic dimension, if there is one. If there is no such intrinsic dimension, it becomes the corresponding dimension of the background positioning area.
      - **Background sizing for vector images that lack intrinsic dimensions or proportions is not yet fully implemented in all browsers.**
  ```css
  /* Keyword values */
  background-size: cover;
  background-size: contain;
  
  /* One-value syntax */
  /* the width of the image (height becomes 'auto') */
  background-size: 50%;
  background-size: 3.2em;
  background-size: 12px;
  background-size: auto;
  
  /* Two-value syntax */
  /* first value: width of the image, second value: height */
  background-size: 50% auto;
  background-size: 3em 25%;
  background-size: auto 6px;
  background-size: auto auto;
  
  /* Multiple backgrounds */
  background-size: auto, auto; /* Not to be confused with `auto auto` */
  background-size: 50%, 25%, 25%;
  background-size: 6px, auto, contain;
  
  /* Global values */
  background-size: inherit;
  background-size: initial;
  background-size: revert;
  background-size: unset;
  ```

#### Positioning the background image

- Background-position is a shorthand for background-position-x and background-position-y, which allow you to set the different axis position values individually.
- The background-position property allows you to choose the position in which the background image appears on the box it is applied to. This uses a coordinate system in which the top-left-hand corner of the box is (0,0), and the box is positioned along the horizontal (x) and vertical (y) axes.
- The default background-position value is (0,0).
- You can use keywords, length, percentages, mixed values, and a 4-value syntax in order to indicate a distance from certain edges of the box — the length unit, in this case, is an offset from the value that precedes it.
  ```css
  /* Keyword values */
  background-position: top;
  background-position: bottom;
  background-position: left;
  background-position: right;
  background-position: center;
  
  /* <percentage> values */
  background-position: 25% 75%;
  
  /* <length> values */
  background-position: 0 0;
  background-position: 1cm 2cm;
  
  /* Multiple images */
  background-position: 0 0, center;
  
  /* Edge offsets values */
  /* Below we are positioning the background 10px from the bottom and 20px from the right:*/
  background-position: bottom 10px right 20px;
  background-position: top right 10px;
  
  /* Global values */
  background-position: inherit;
  background-position: initial;
  background-position: revert;
  background-position: unset;
  ```

#### Gradients

- A gradient — when used for a background — acts just like an image and is also set by using the background-image property.
- The <gradient> CSS data type is a special type of <image> that consists of a progressive transition between two or more colors.
- A CSS gradient has no intrinsic dimensions; i.e., it has no natural or preferred size, nor a preferred ratio. Its concrete size will match the size of the element to which it applies.
- The `<gradient>` data type is defined with one of the function types:
  - **Linear gradient**: Linear gradients transition colors progressively along an imaginary line. They are generated with the `linear-gradient()` function.
  - **Radial gradient**: Radial gradients transition colors progressively from a center point (origin). They are generated with the `radial-gradient()` function.
  - **Repeating gradient**: Repeating gradients duplicate a gradient as much as necessary to fill a given area. They are generated with the `repeating-linear-gradient()` and repeating-radial-gradient() functions.
  - **Conic gradient**: Conic gradients transition colors progressively around a circle. They are generated with the `conic-gradient()` function.
- As with any interpolation involving colors, gradients are calculated in the alpha-premultiplied color space. This prevents unexpected shades of gray from appearing when both the color and the opacity are changing.
- Gradients can be happily mixed with regular background images.

#### Multiple background images

- It is also possible to have multiple background images — you specify multiple background-image values in a single property value, separating each one with a comma.
- The other `background-*` properties can also have comma-separated values in the same way.
- Each value of the different properties will match up to the values in the same position in the other properties.
- When different properties have different numbers of values, the smaller numbers of values will cycle.

#### Background attachment

- The background-attachment CSS property sets whether a background image's position is fixed within the viewport, or scrolls with its containing block. (**Specifies how backgrounds scroll when the content scrolls.**) 
- **The background-attachment property only has an effect when there is content to scroll.**
- **It supports multiple backgrounds.** You can specify a different <attachment> for each background, separated by commas. Each image is matched with the corresponding <attachment> type, from first specified to last.
- Available values:
  - **fixed**: The background is fixed relative to the viewport. Even if an element has a scrolling mechanism, the background doesn't move with the element. (This is not compatible with background-clip: text.)
  - **local**: The background is fixed relative to the element's contents. If the element has a scrolling mechanism, the background scrolls with the element's contents, and the background painting area and background positioning area are relative to the scrollable area of the element rather than to the border framing them.
  - **scroll**: The background is fixed relative to the element itself and does not scroll with its contents. (It is effectively attached to the element's border.)

  ```css
  /* Keyword values */
  background-attachment: scroll;
  background-attachment: fixed;
  background-attachment: local;
  
  /* Global values */
  background-attachment: inherit;
  background-attachment: initial;
  background-attachment: revert;
  background-attachment: unset;
  ```

## Borders

- The border shorthand CSS property sets an element's border. It sets the values of `border-width`, `border-style`, and `border-color`. **The order of the values does not matter.**
- Top, right, bottom, and left border properties also have mapped logical properties that relate to the writing mode of the document (e.g. left-to-right or right-to-left text, or top-to-bottom).
- The border will be invisible if its style is not defined. This is because the style defaults to none.
- Values:
  - `<line-width>`
    - Defines the width of the border, either as an explicit nonnegative `<length>` or a keyword. Defaults to medium if absent.
    - If it's a `keyword`, it must be one of the following values:
      - `thin`
      - `medium`
      - `thick`
    - The border-width property may be specified using one, two, three, or four values.
      - **When one value is specified**, it applies the same width to all four sides.
      - **When two values are specified**, the first width applies to the top and bottom, the second to the left and right.
      - **When three values are specified**, the first width applies to the top, the second to the left and right, the third to the bottom.
      - **When four values are specified**, the widths apply to the top, right, bottom, and left in that order (**clockwise**).
    - Because the specification doesn't define the exact thickness denoted by each keyword, the precise result when using one of them is implementation-specific. Nevertheless, they always follow the pattern `thin ≤ medium ≤ thick`, and the values are constant within a single document.
  - `<line-style>`
    - The border-style shorthand CSS property sets the line style for all four sides of an element's border. **Defaults to none if absent.**
    - The border-style property may be specified using one, two, three, or four values. It works the same way as it was at the border-width. 
    - Available options:
      - `none`: Like the hidden keyword, displays no border. Unless a background-image is set, the computed value of the same side's border-width will be 0, even if the specified value is something else. In the case of table cell and border collapsing, the none value has the lowest priority: if any other conflicting border is set, it will be displayed.
      - `hidden`: Like the none keyword, displays no border. Unless a background-image is set, the computed value of the same side's border-width will be 0, even if the specified value is something else. In the case of table cell and border collapsing, the hidden value has the highest priority: if any other conflicting border is set, it won't be displayed.
      - `dotted`: Displays a series of rounded dots. The spacing of the dots is not defined by the specification and is implementation-specific. The radius of the dots is half the computed value of the same side's border-width.
      - `dashed`: Displays a series of short square-ended dashes or line segments. The exact size and length of the segments are not defined by the specification and are implementation-specific.
      - `solid`: Displays a single, straight, solid line.
      - `double`: Displays two straight lines that add up to the pixel size defined by border-width.
      - `groove`: Displays a border with a carved appearance. It is the opposite of ridge.
      - `ridge`: Displays a border with an extruded appearance. It is the opposite of groove.
      - `inset`: Displays a border that makes the element appear embedded. It is the opposite of outset. When applied to a table cell with border-collapse set to collapsed, this value behaves like groove.
      - `outset`: Displays a border that makes the element appear embossed. It is the opposite of inset. When applied to a table cell with border-collapse set to collapsed, this value behaves like ridge.
  - `<color>`
- As with all shorthand properties, any omitted sub-values will be set to their initial value. Importantly, border cannot be used to specify a custom value for border-image, but instead sets it to its initial value, i.e., none.
- Borders and outlines are very similar. However, outlines differ from borders in the following ways:
  - Outlines never take up space, as they are drawn outside of an element's content.
  - According to the spec, outlines don't have to be rectangular, although they usually are.

```css
/* style */
border: solid;

/* width | style */
border: 2px dotted;

/* style | color */
border: outset #f33;

/* width | style | color */
border: medium dashed green;

/* Global values */
border: inherit;
border: initial;
border: unset;
```
  
#### Rounded corners

- Rounding corners on a box is achieved by using the border-radius property and associated longhands which relate to each corner of the box.
- Two lengths or percentages may be used as a value, the first value defining the horizontal radius, and the second the vertical radius.
- The radius applies to the whole background, even if the element has no border; the exact position of the clipping is defined by the `background-clip` property.
- **The border-radius property does not apply to table elements when border-collapse is collapse.**
- The border-radius property is specified as:
  - one, two, three, or four `<length>` or `<percentage>` values. This is used to set a single radius for the corners.
  - followed optionally by `/` and one, two, three, or four `<length>` or `<percentage>` values. This is used to set an additional radius, so you can have elliptical corners.

### Border image

- The border-image CSS property draws an image around a given element. It replaces the element's regular border.
- **If the computed value of border-image-source is none, or if the image cannot be displayed, the border-style will be displayed instead.** Indeed, this is required according to the specification, although not all browsers implement this requirement.
- Values:
  - `border-image-source`: The source image. See border-image-source.
  - `border-image-slice`: The dimensions for slicing the source image into regions. Up to four values may be specified. See border-image-slice.
  - `border-image-width`: The width of the border image. Up to four values may be specified. See border-image-width.
  - `border-image-outset`: The distance of the border image from the element's outside edge. Up to four values may be specified. See border-image-outset.
  - `border-image-repeat`: Defines how the edge regions of the source image are adjusted to fit the dimensions of the border image. Up to two values may be specified. See border-image-repeat.

```css
/* source | slice */
border-image: linear-gradient(red, blue) 27;

/* source | slice | repeat */
border-image: url("/images/border.png") 27 space;

/* source | slice | width */
border-image: linear-gradient(red, blue) 27 / 35px;

/* source | slice | width | outset | repeat */
border-image: url("/images/border.png") 27 23 / 50px 30px / 1rem round space;

/* Global values */
border-image: inherit;
border-image: initial;
border-image: revert;
border-image: unset;

```

### Outlines vs borders

- Borders and outlines are very similar. However, outlines differ from borders in the following ways:
  - Outlines never take up space, as they are drawn outside of an element's content, and may overlap other content.
  - According to the spec, outlines don't have to be rectangular, although they usually are.
- The outline CSS shorthand property set all the outline properties in a single declaration.
- The `outline` property is a shorthand for the following CSS properties:
  - `outline-color`:
    - The outline-color property is specified as a `<color>` or the keyword `invert`, which performs a color inversion of the background. Note that browsers are not required to support this value; if they don't, this keyword is considered invalid.
  - `outline-style`:
    - The outline-style CSS property sets the style of an element's outline. An outline is a line that is drawn around an element, outside the border.
    - The outline-style property is specified as any one of the values listed below:
      - `auto`: Permits the user agent to render a custom outline style.
      - `none`: No outline is used. The outline-width is 0.
      - `dotted`: The outline is a series of dots.
      - `dashed`: The outline is a series of short line segments.
      - `solid`: The outline is a single line.
      - `double`: The outline is two single lines. The outline-width is the sum of the two lines and the space between them.
      - `groove`: The outline looks as though it were carved into the page.
      - `ridge`: The opposite of groove: the outline looks as though it were extruded from the page.
      - `inset`: The outline makes the box look as though it were embedded in the page.
      - `outset`: The opposite of inset: the outline makes the box look as though it were coming out of the page.
  - `outline-width`:
    - The CSS outline-width property sets the thickness of an element's outline. An outline is a line that is drawn around an element, outside the border.
    - The outline-width property is specified as any one of the values listed below.
      - `<length>`: The width of the outline specified as a `<length>`.
      - `thin`: Depends on the user agent. Typically equivalent to 1px in desktop browsers (including Firefox).
      - `medium`: Depends on the user agent. Typically equivalent to 3px in desktop browsers (including Firefox).
      - `thick`: Depends on the user agent. Typically equivalent to 5px in desktop browsers (including Firefox).
- The outline will be invisible for many elements if its style is not defined. This is because the style defaults to `none`. **A notable exception is input elements, which are given default styling by browsers.**
- The outline property may be specified using one, two, or three of the values listed below. The order of the values does not matter.
  ```css
  /* style */
  outline: solid;
  
  /* color | style */
  outline: #f66 dashed;
  
  /* style | width */
  outline: inset thick;
  
  /* color | style | width */
  outline: green solid 3px;
  
  /* Global values */
  outline: inherit;
  outline: initial;
  outline: revert;
  outline: unset;
  ```
## Writing modes

- In recent years, CSS has evolved in order to better support different directionality of content, including right-to-left but also top-to-bottom content (such as Japanese) — these different directionalities are called writing modes.
- **A writing mode in CSS refers to whether the text is running horizontally or vertically.**
- You don't need to be working in a language which uses a vertical writing mode to want to do this — you could also change the writing mode of parts of your layout for creative purposes.
- The `writing-mode` property lets us switch from one writing mode to another.
  - **This property specifies the block flow direction, which is the direction in which block-level containers are stacked, and the direction in which inline-level content flows within a block container.** Thus, it also determines the ordering of block-level content.
  - When set for an entire document, it should be set on the root element (html element for HTML documents).
  - Available values:
    - `horizontal-tb`: For ltr scripts, content flows horizontally from left to right. For rtl scripts, content flows horizontally from right to left. The next horizontal line is positioned below the previous line.
    - `vertical-rl`: For ltr scripts, content flows vertically from top to bottom, and the next vertical line is positioned to the left of the previous line. For rtl scripts, content flows vertically from bottom to top, and the next vertical line is positioned to the right of the previous line.
    - `vertical-lr`: For ltr scripts, content flows vertically from top to bottom, and the next vertical line is positioned to the right of the previous line. For rtl scripts, content flows vertically from bottom to top, and the next vertical line is positioned to the left of the previous line.
    - `sideways-rl`[experimental]: For ltr scripts, content flows vertically from bottom to top. For rtl scripts, content flows vertically from top to bottom. All the glyphs, even those in vertical scripts, are set sideways toward the right.
    - `sideways-lr`[experimental]: For ltr scripts, content flows vertically from top to bottom. For rtl scripts, content flows vertically from bottom to top. All the glyphs, even those in vertical scripts, are set sideways toward the left.
    - *There are several deprecated values which are available for SVG1 documents only (lr, lr-tb, rl, tb, tb-rl, tb-lr).*

### Writing modes and block and inline layout

- **Block and inline is tied to the writing mode of the document, and not the physical screen.**
- Blocks are only displayed from the top to the bottom of the page if you are using a writing mode that displays text horizontally, such as English.
- When we switch the writing mode, we are changing which direction is block and which is inline.
- In a horizontal-tb writing mode the block direction runs from top to bottom; in a vertical-rl writing mode the block direction runs right-to-left horizontally. So the **block dimension** is always the direction blocks are displayed on the page in the writing mode in use. The **inline dimension** is always the direction a sentence flows.
- Dimensions when in a horizontal writing mode:
  
  ![dimensions when in a horizontal writing mode](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Handling_different_text_directions/horizontal-tb.png)
  
- Dimensions in a vertical writing mode:
  
  ![dimensions in a vertical writing mode](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Handling_different_text_directions/vertical.png)

### Direction

- Due to the fact that writing mode and direction of text can change, newer CSS layout methods do not refer to left and right, and top and bottom. Instead they will talk about start and end along with this idea of inline and block.
- The property mapped to width when in a horizontal writing mode is called **inline-size** — it refers to the size in the inline dimension.
- The property for height is named **block-size** and is the size in the block dimension.

### Logical margin, border, and padding properties

- Physical values of `top`, `right`, `bottom`, and `left` have mappings, to logical values — `block-start`, `inline-end`, `block-end`, and `inline-start`.

  ```css
  /**
    The 2 examples below are identical.
  */
  .logical {
    margin-block-start: 20px;
    padding-inline-end: 2em;
    padding-block-start: 2px;
    border-block-start: 5px solid pink;
    border-inline-end: 10px dotted rebeccapurple;
    border-block-end: 1em double orange;
    border-inline-start: 1px solid black;
  }
  
  .physical {
    margin-top: 20px;
    padding-right: 2em;
    padding-top: 2px;
    border-top: 5px solid pink;
    border-right: 10px dotted rebeccapurple;
    border-bottom: 1em double orange;
    border-left: 1px solid black;
  }
  ```

- Currently, only Firefox supports flow relative values  for float. If you are using Google Chrome or Microsoft Edge, you may find that the image did not float.

## Overflow

- Overflow happens when there is too much content to fit in a box.
- Wherever possible, CSS does not hide content. This would cause data loss. Instead, CSS overflows in visible ways.
- The  `overflow` property is how you take control of an element's overflow. The default value of overflow is `visible`.
  - The overflow property is specified as one or two keywords chosen from the list of values below. If two keywords are specified, the first applies to `overflow-x` and the second to `overflow-y`. Otherwise, both `overflow-x` and `overflow-y` are set to the same value.
  - Values:
    - `visible`: Content is not clipped and may be rendered outside the padding box.
    - `hidden`: Content is clipped if necessary to fit the padding box. No scrollbars are provided, and no support for allowing the user to scroll (such as by dragging or using a scroll wheel) is allowed. The content can be scrolled programmatically (for example, by setting the value of a property such as offsetLeft), so the element is still a scroll container.
    - `clip`: Similar to hidden, the content is clipped to the element's padding box. The difference between clip and hidden is that the clip keyword also forbids all scrolling, including programmatic scrolling. The box is not a scroll container, and does not start a new formatting context. If you wish to start a new formatting context, you can use display: flow-root to do so.
    - `scroll`: Content is clipped if necessary to fit the padding box. Browsers always display scrollbars whether or not any content is actually clipped, preventing scrollbars from appearing or disappearing as content changes. Printers may still print overflowing content.
    - `auto`: Depends on the user agent. If content fits inside the padding box, it looks the same as visible, but still establishes a new block formatting context. Desktop browsers provide scrollbars if content overflows.
    - `overlay` (**DEPRECATED**): Behaves the same as auto, but with the scrollbars drawn on top of content instead of taking up space. Only supported in WebKit-based (e.g., Safari) and Blink-based (e.g., Chrome or Opera) browsers.
  - Specifying a value other than visible (the default) or clip creates a new block formatting context. This is necessary for technical reasons — if a float intersected with the scrolling element it would forcibly rewrap the content after each scroll step, leading to a slow scrolling experience.
  - Setting one axis to visible (the default) while setting the other to a different value results in visible behaving as auto.
  - In order for overflow to have an effect, the block-level container must have either a set height (`height` or `max-height`) or `white-space` set to `nowrap`.
  - The JavaScript `Element.scrollTop` property may be used to scroll an HTML element even when overflow is set to `hidden`.
  - Using `overflow: scroll`, browsers with visible scrollbars will always display them—even if there is not enough content to overflow.
  - If **two keywords are specified**, the first applies to overflow-x and the second applies to overflow-y. Otherwise, both overflow-x and overflow-y are set to the same value.
  - When you use a value of overflow such as `scroll` or `auto`, you create a **Block Formatting Context** (BFC). **The content of the box that you have changed the value of overflow for acquires a self-contained layout.** Content outside the container cannot poke into the container, and nothing can poke out of that container into the surrounding layout. **This enables scrolling behavior, as all box content needs to be contained and not overlap, in order to create a consistent scrolling experience.**

### Wrapping and breaking text

- In CSS, if you have an **unbreakable string such as a very long word, by default it will overflow any container** that is too small for it in the inline direction. CSS will display overflow in this way, **because doing something else could cause data loss.**
- To find the minimum size of the box that will contain its contents with no overflows, set the `width` or `inline-size` property of the box to `min-content`.
- Using `min-content` is therefore one possibility for overflowing boxes. If it is possible to allow the box to grow to be the minimum size required for the content, but no bigger

#### Breaking long words

- **If the box needs to be a fixed size**, or you are keen to ensure that long words can't overflow, **then `overflow-wrap` property will break a word once it is too long to fit** on a line by itself.
  - In contrast to `word-break`, **`overflow-wrap` will only create a break if an entire word cannot be placed on its own line without overflowing**.
  - *The property was originally a nonstandard and unprefixed Microsoft extension called word-wrap, and was implemented by most browsers with the same name. It has since been renamed to overflow-wrap, with word-wrap being an alias.*
  - The overflow-wrap property is specified as a single keyword:
    - `normal`: **Lines may only break at normal word break points** (such as a space between two words).
    - `anywhere`: **To prevent overflow, an otherwise unbreakable string of characters** — like a long word or URL — **may be broken at any point** if there are no otherwise-acceptable break points in the line. No hyphenation character is inserted at the break point. Soft wrap opportunities introduced by the word break are considered when calculating min-content intrinsic sizes.
    - `break-word`: **The same as the anywhere value**, with normally unbreakable words allowed to be broken at arbitrary points if there are no otherwise acceptable break points in the line, **but soft wrap opportunities introduced by the word break are NOT considered when calculating min-content intrinsic sizes**.

  ```css
  /* Keyword values */
  overflow-wrap: normal;
  overflow-wrap: break-word;
  overflow-wrap: anywhere;

  /* Global values */
  overflow-wrap: inherit;
  overflow-wrap: initial;
  overflow-wrap: revert;
  overflow-wrap: unset;
  ```

- The `word-break` property will break the word at the point it overflows. It **will cause a break-even if placing the word onto a new line would allow it to display without breaking**.
- The word-break property is specified as a single keyword:
  - `normal`: Use the default line break rule.
  - `break-all`: To prevent overflow, word breaks should be inserted between any two characters (excluding Chinese/Japanese/Korean text).
  - `keep-all`: Word breaks should not be used for Chinese/Japanese/Korean (CJK) text. Non-CJK text behavior is the same as for normal.
  - `break-word` (**DEPRECATED**): Has the same effect as word-break: normal and overflow-wrap: anywhere, regardless of the actual value of the overflow-wrap property.
- In contrast to `word-break: break-word` and `overflow-wrap: break-word`, **`word-break: break-all` will create a break at the exact place where text would otherwise overflow its container (even if putting an entire word on its own line would negate the need for a break)**.

  ```css
  /* Keyword values */
  word-break: normal;
  word-break: break-all;
  word-break: keep-all;
  word-break: break-word; /* deprecated */

  /* Global values */
  word-break: inherit;
  word-break: initial;
  word-break: revert;
  word-break: unset;
  ```

#### Adding hyphens

- **The `hyphens` CSS property specifies how words should be hyphenated when text wraps across multiple lines**. It can prevent hyphenation entirely, hyphenate at manually-specified points within the text, or let the browser automatically insert hyphens where appropriate.
- **The `&shy;` character is used to indicate a potential place to insert a hyphen when `hyphens: manual;` is specified.**
- **Hyphenation rules are language-specific.** In HTML, the language is determined by the lang attribute, and browsers will hyphenate only if this attribute is present and the appropriate hyphenation dictionary is available. *In XML, the xml:lang attribute must be used.*
- **The rules defining how hyphenation is performed are not explicitly defined by the specification, so the exact hyphenation may vary from browser to browser.**
- The hyphens property is specified as a single keyword value:
  - `none`: Words are not broken at line breaks, even if characters inside the words suggest line break points. Lines will only wrap at whitespace.
  - `manual`: Words are broken for line-wrapping only where characters inside the word suggest line break opportunities. See Suggesting line break opportunities below for details.
  - `auto`: The browser is free to automatically break words at appropriate hyphenation points, following whatever rules it chooses. However, suggested line break opportunities (see Suggesting line break opportunities below) will override automatic break point selection when present.
    - The auto setting's behavior depends on the language being properly tagged to select the appropriate hyphenation rules. You must specify a language using the lang HTML attribute to guarantee that automatic hyphenation is applied in that language.

  ```css
    /* Keyword values */
    hyphens: none;
    hyphens: manual;
    hyphens: auto;

    /* Global values */
    hyphens: inherit;
    hyphens: initial;
    hyphens: revert;
    hyphens: unset;
  ```

#### Suggesting line break opportunities

- There are two Unicode characters used to manually specify potential line break points within text:
  - **`U+2010 (HYPHEN)`**: The **"hard" hyphen character** indicates a visible line break opportunity. Even if the line is not actually broken at that point, the hyphen is still rendered.
  - **`U+00AD (SHY)`**: An **invisible, "soft" hyphen.** This character is **not rendered visibly; instead, it marks a place where the browser should break the word if hyphenation is necessary**. In HTML, use `&shy;` to insert a soft hyphen.

#### The `<wbr>` element
  - **The `<wbr>` HTML element represents a word break opportunity—a position within text where the browser may optionally break a line**, though its line-breaking rules would not otherwise create a break at that location.
  - **The element does not introduce a hyphen at the line break point.**
  - *This element was first implemented in Internet Explorer 5.5 and was officially defined in HTML5.*
