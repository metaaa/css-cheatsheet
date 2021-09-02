### This article is just an extract of [this](https://developer.mozilla.org/en-US/docs/Web/CSS/Syntax) superb article from Mozilla.

## What CSS is?
- CSS stands for **Cascading Style Sheets**

## Basics
- **Both properties and values are case-insensitive** by default in CSS.
- **The pair is separated by a colon**, ':' (U+003A COLON), **and white spaces** before, between, and after properties and values, but not necessarily inside, **are ignored**.
- **Declarations are grouped in blocks, that is in a structure delimited by an opening brace,** '{' (U+007B LEFT CURLY BRACKET), **and a closing one**, '}' (U+007D RIGHT CURLY BRACKET).
- Such blocks are naturally called declaration blocks and **declarations** inside them **are separated by a semi-colon,** ';' (U+003B SEMICOLON).
- **Each (valid) declaration block is preceded by one or more comma-separated selectors**, which are conditions selecting some elements of the page. **A selector group and an associated declarations block, together, are called a ruleset, or often a rule**.
- **As an element of the page may be matched by several selectors**, and therefore by several rules potentially containing a given property several times, with different values, **the CSS standard defines which one has precedence over the other and must be applied: this is called the cascade algorithm.
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
    h1 | 0 | 0 | 0 | 1 | 0001
    h1 + p::first-letter | 0 | 0 | 0 | 3 | 0003
    li > a[href*="en-US"] > .inline-warning | 0 | 0 | 2 | 2 | 0022
    #identifier | 0 | 1 | 0 | 0 | 0100
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
  [attr] | a[title] | Matches elements with an attr attribute (whose name is the value in square brackets).
  [attr=value] | a[href="https://example.com"] | Matches elements with an attr attribute whose value is exactly value — the string inside the quotes.
  [attr~=value] | p[class~="special"] | Matches elements with an attr attribute whose value is exactly value, or contains value in its (space separated) list of values.
  [attr&#124;=value] | div[lang&#124;="zh"] | Matches elements with an attr attribute whose value is exactly value or begins with value immediately followed by a hyphen.
  
- substring matching selectors (part of attr selectors)
  - These selectors allow for more advanced matching of substrings inside the value of your attribute.

  Selector | Example | Description
  -------- | ------- | -----------
  [attr^=value] | li[class^="box-"] | Matches elements with an attr attribute, whose value begins with value.
  [attr$=value] | li[class$="-box"] | Matches elements with an attr attribute whose value ends with value.
  [attr*=value] | li[class*="box"] | Matches elements with an attr attribute whose value contains value anywhere within the string.
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
  :active | Matches when the user activates (for example clicks on) an element.
  :any-link | Matches both the :link and :visited states of a link.
  :blank | Matches an <input> element whose input value is empty.
  :checked | Matches a radio button or checkbox in the selected state.
  :current | Matches the element, or an ancestor of the element, that is currently being displayed.
  :default | Matches the one or more UI elements that are the default among a set of similar elements.
  :dir | Select an element based on its directionality (value of the HTML dir attribute or CSS direction property).
  :disabled | Matches user interface elements that are in an disabled state.
  :empty | Matches an element that has no children except optionally white space.
  :enabled | Matches user interface elements that are in an enabled state.
  :first | In Paged Media, matches the first page.
  :first-child | Matches an element that is first among its siblings.
  :first-of-type | Matches an element which is first of a certain type among its siblings.
  :focus | Matches when an element has focus.
  :focus-visible | Matches when an element has focus and the focus should be visible to the user.
  :focus-within | Matches an element with focus plus an element with a descendent that has focus.
  :future | Matches the elements after the current element.
  :hover | Matches when the user hovers over an element.
  :indeterminate | Matches UI elements whose value is in an indeterminate state, usually checkboxes.
  :in-range | Matches an element with a range when its value is in-range.
  :invalid | Matches an element, such as an <input>, in an invalid state.
  :lang | Matches an element based on language (value of the HTML lang attribute).
  :last-child | Matches an element which is last among its siblings.
  :last-of-type | Matches an element of a certain type that is last among its siblings.
  :left | In Paged Media, matches left-hand pages.
  :link | Matches unvisited links.
  :local-link | Matches links pointing to pages that are in the same site as the current document.
  :is() | Matches any of the selectors in the selector list that is passed in.
  :not | Matches things not matched by selectors that are passed in as a value to this selector.
  :nth-child | Matches elements from a list of siblings — the siblings are matched by a formula of the form an+b (e.g. 2n + 1 would match elements 1, 3, 5, 7, etc. All the odd ones.)
  :nth-of-type | Matches elements from a list of siblings that are of a certain type (e.g. <p> elements) — the siblings are matched by a formula of the form an+b (e.g. 2n + 1 would match that type of element, numbers 1, 3, 5, 7, etc. All the odd ones.)
  :nth-last-child | Matches elements from a list of siblings, counting backwards from the end. The siblings are matched by a formula of the form an+b (e.g. 2n + 1 would match the last element in the sequence, then two elements before that, then two elements before that, etc. All the odd ones, counting from the end.)
  :nth-last-of-type | Matches elements from a list of siblings that are of a certain type (e.g. <p> elements), counting backwards from the end. The siblings are matched by a formula of the form an+b (e.g. 2n + 1 would match the last element of that type in the sequence, then two elements before that, then two elements before that, etc. All the odd ones, counting from the end.)
  :only-child | Matches an element that has no siblings.
  :only-of-type | Matches an element that is the only one of its type among its siblings.
  :optional | Matches form elements that are not required.
  :out-of-range | Matches an element with a range when its value is out of range.
  :past | Matches the elements before the current element.
  :placeholder-shown | Matches an input element that is showing placeholder text.
  :playing | Matches an element representing an audio, video, or similar resource that is capable of being “played” or “paused”, when that element is “playing”.
  :paused | Matches an element representing an audio, video, or similar resource that is capable of being “played” or “paused”, when that element is “paused”.
  :read-only | Matches an element if it is not user-alterable.
  :read-write | Matches an element if it is user-alterable.
  :required | Matches form elements that are required.
  :right | In Paged Media, matches right-hand pages.
  :root | Matches an element that is the root of the document.
  :scope | Matches any element that is a scope element.
  :valid | Matches an element such as an <input> element, in a valid state.
  :target | Matches an element if it is the target of the current URL (i.e. if it has an ID matching the current URL fragment).
  :visited | Matches visited links.

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
  ::after | Inserts a stylable element appearing after the originating element's actual content, if used with the content property with a value other than none.
  ::before | Inserts a stylable element appearing before the originating element's actual content, if used with the content property with a value other than none.
  ::first-letter | Matches the first letter of the element.
  ::first-line | Matches the first line of the containing element.
  ::grammar-error | Matches a portion of the document containing a grammar error as flagged by the browser.
  ::marker | Matches the marker box of a list item, which typically contains a bullet or number.
  ::selection | Matches the portion of the document that has been selected.
  ::spelling-error | Matches a portion of the document containing a spelling error as flagged by the browser.
    
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
  - Outer display types:
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
  - Inner display types:
    - It dictates how elements inside that box are laid out.
    - By default, the elements inside a box are laid out in normal flow, which means that they behave just like any other block and inline elements
  - **display: inline-block:**
    - Is a special value of display, which provides a middle ground between inline and block.
    - This is useful for situations where you do not want an item to break onto a new line, but do want it to respect width and height and avoid the overlapping seen above.
    - *This can be useful when you want to give a link a larger hit area by adding padding. `<a>` is an inline element like <span>; you can use display: inline-block to allow padding to be set on it, making it easier for a user to click the link.*
- **The CSS box model as a whole applies to block boxes. Inline boxes use just some of the behavior defined in the box model.** The model defines how the different parts of a box — margin, border, padding, and content — work together to create a box that you can see on a page.
- Parts of a box:
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
  ![parts of a box](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/The_box_model/box-model.png)
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
