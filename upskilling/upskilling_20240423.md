# The CSS Style Computation Process

The CSS style computation process happens for every single CSS property. It is the process used to calculate what the final value of a specific CSS property should be. It is the "cascading" behaviour of CSS.

The process consists of four steps:

1. Finding the CSS properties without conflicts
2. Finding and resolving the CSS properties with conflicts
3. Using inherited values
4. Using the default value

We can use the following example to understand:

```html
<div>
    <h1 class="text">Lorem</h1>
</div>
```

```css
.red { 
    color: red;
    font-size: 40px;
}

h1 {
    font-size: 26px;
}

div h1.red {
    font-size: 3em;
    font-size: 30px;
}

/* Browser default styles */
h1 {
    display: block;
    font-size: 2em;
    font-weight: bold;
}
```

## Finding the CSS properties without conflicts

In this step, we simply find the values which do not have conflicts. In our case:

```css
/* .red */
color: red;

/* browser defaults */
display: block;
font-weight: bold;
```

## Finding and resolving the CSS properties with conflicts

In this step, we find the CSS properties which have conflicts. There are three steps to this:
1. Comparing CSS origin
2. Comparing CSS specificity
3. Comparing the order which they appear in the source code.

In our example, the styles with conflicts are:

```css
.red { 
    font-size: 40px;
}

h1 {
    font-size: 26px;
}

div h1.red {
    font-size: 3em;
    font-size: 30px;
}

/* Browser default styles */
h1 {
    font-size: 2em;
}
```

### Comparing the CSS origin

- The user agent stylesheet (browser default styles) have lowest importance.
- The author stylesheets (CSS written by the author of the website) has higher importance.

Following these rules, we can immediately eliminate the user agent styles, resulting in this:

```css
.red { 
    font-size: 40px;
}

h1 {
    font-size: 26px;
}

div h1.red {
    font-size: 3em;
    font-size: 30px;
}
```

### Comparing CSS Specificity

CSS Specificity is a concept linked to the way a CSS selector is written. It is a number which can be broken into four parts (a,b,c,d), from left to right in order of importance:
- Whether or not the style is an inline style (attached directly to the element)
- ID selectors
- Class and pseudo-class selectors
- Element and pseudo-element selectors

For example, the inline style: `<div style="color: red"></div>` will result in a specificity of (1,0,0,0)

Another example of specificity within CSS selectors:

```css
/* Specificity: (0, 1, 0, 0) */
.red { } 

/* Specificity: (0,3,2,5) */
h1.a div#b.c p#d aside i { }
```

The same property definition from selectors with a higher specificity will always override the same property definition from a rule with lower specificity.

### Comparing the order which they appear in the source code.

If at this case, the conflicts still haven't been resolved, then the browser will use the order which they are written in the source code. The rule which appears last in the source code will be used as the final style.

## Using inherited values

If in this step, a value is still not set or resolved, the browser will try to inherit the value from its parent element. Some properties in CSS may be inherited, such as `color`, but some may not, such as `margin` and `float`. For the properties which can be inherited, CSS will try to use inheritance.

## Using the default value

If finally, no value has been given to a specific value, then the browser will use the default value for that property. All CSS properties have a default value, and that will be what is used here.

# Upskilling Artefacts

In this upskilling, I worked on the a small project within the `小米首页/` folder. 
- 5h on the 24th
- 3h on the 26th
