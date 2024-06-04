# Upskilling 18-04-2024

## What is HTML and CSS?

HTML is a markup language, used to describe the structure of the contents of a webpage.
- Latest version is HTML5 (H5)
- HTML follows an XML-like syntax
- HTML consists of tags/elements
- HTML stands for **Hypertext Markup Language**
- HTML tags are not case sensitive

CSS is a language used to dictate how a web page is meant to look.
- Latest version is CSS3
- CSS syntax consists of:
    - CSS Selectors (used to select specific elements to apply styles to)
    - CSS Rules (used to dictate what the styles are to be applied)

The above contribute to a single concept in CSS called CSS Statements (selector + rule)

## Are there any options other than HTML and CSS?

There is an alternative to HTML called XHTML, which is a language used to write HTML in a way which strictly follows the XML language specifications.

CSS doesn't have any alternatives - however, there are multiple ways to writing CSS code.

## HTML Attributes

All HTML elements may have attributes applied to them. Some are inbuilt attributes, which are specified by the WHATWG specifications, and these attributes may allow the browser to interpret them in different ways. Other attributes, which are not defined in the WHATWG specifications, are called "user-defined attributes", which do not have any impact on the element's behaviour, nor does it have an impact on how the browser interprets those elements.

Attributes in HTML follow the syntax for specifying attributes in XML. For example, an attribute would look like: `attributeName="<attribute_value>"`

Some common attributes include the following:
- `id`: a unique ID in the current webpage for that current element
- `class`: a way of grouping common elements together
- `title`: a title for the element, displayed when the element is hovered on

## HTML Tree-like Structure

HTML code resembles a tree-like structure. Each element may have ancestral elements (elements which contain the current element), and may also have descendant elements (elements which are contained by the current element).

Given this example:

```html
<div id="div-1">
    <div id="div-2">
        <div id="div-3">
        </div>
    </div>
</div>
```

We can say the following:
- `div-2` and `div-3` are descendants of `div-1`, with `div-2` being a direct descendant of `div-1` (or called a "child element")
- `div-1` is an ancestor of `div-2` (also the parent, as it is the direct ancestor)
- `div-3` is a descendant of `div-2` (also a child, as it is a direct descendant)
- `div-1` and `div-2` are ancestors of `div-3`, with `div-2` being the parent of `div-3`.

## HTML Semantics

HTML Semantics are an important part of web development to allow your web page to be strategically studied by web crawlers, in order to easily summarize your web page's content to allow search engines to rank your page higher in the search results. It is a part of a larger topic called Search Engine Optimization (SEO).

HTML Semantics mean that each HTML element has its own meaning to it, for example:
- `p` is a paragraph
- `aside` is extra information
- `address` is an address information
- `a` is a hyperlink to another location

However, some tags may not have any semantics or meaning to it, for example:
- `div`
- `span`
- `i`

So, given a block of code like this:

```html
<div>
    Some text some long paragraph of text i have long text
    <div>
        i am some extra information
    </div>
</div>
```

An improvement using better HTML semantics would be like the following:

```html
<p>
    Some text some long paragraph of text i have long text
    <aside>
        i am some extra information
    </aside>
</p>
```

## HTML Comments

Just like any language, HTML also has comments. The symbols for commenting in HTML are as the following:
- `<!--` denotes the start of a comment
- `-->` denotes the end of a comment,

... and any text placed inside will be commented text. An example is as follows:

```html
<!-- This is a paragraph of some long text -->
<p>
    Some text some long paragraph of text i have long text

    <!-- This is some extra information -->
    <aside>
        i am some extra information
    </aside>
</p>
```

## HTML Whitespace Collapsing

In HTML, there is an important principal called **Whitespace Collapsing**. It means that any sequence of whitespace will be converted into a single space when being parsed by the browser.

For example, a text like `hello       world                        !` will turn into `hello world !`

The same applies to whitespace sequences such as new lines:

```html
<p>
    hello
    world

    look


    whitespace!
    !
</p>
```

... will turn into:

```html
<p> hello world look whitespace! ! </p>
```

This behaviour may result in unexpected effects later when styling the page or layouting the page using CSS.
