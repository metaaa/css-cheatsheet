# This article is just an extract of [this](https://developer.mozilla.org/en-US/docs/Web/CSS/Syntax) superb article from Mozilla.

## What CSS is?
- CSS stands for **Cascading Style Sheets**

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
  - The universal selector (*), combinators (+, >, ~, ' '), and negation pseudo-class (:not) have no effect on specificity.

### Inheritance
- **Some CSS property values set on parent elements are inherited by their child elements, and some aren't**.
- Things like widths (as mentioned above), margins, padding, and borders do not inherit.
- **CSS provides four special universal property values for controlling inheritance**. Every CSS property accepts these values.
  - **inherit:** Sets the property value applied to a selected element to be the same as that of its parent element. Effectively, this "turns on inheritance".
  - **initial:** Sets the property value applied to a selected element to the initial value of that property.
  - **unset:** Resets the property to its natural value, which means that if the property is naturally inherited it acts like inherit, otherwise it acts like initial.
  - **revert:** Acts like unset in many cases, however will revert the property to the browser's default styling rather than the defaults applied to that property.
- **The CSS shorthand property all can be used to apply one of these inheritance values to (almost) all properties at once**. Its value can be any one of the inheritance values (inherit, initial, unsetor revert).

## Some general info
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
- 
