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
- descendant combinator
  ```css
  article p { ... }
  ```
- child combinator
  ```css
  article > p { ... }
  ```
- adjacent sibling combinator
  ```css
  h1 + p { ... }
  ```
- general sibling combinator
  ```css
  h1 ~ p { ... }
  ```
