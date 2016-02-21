# Elysium CSS Guide
- Author: Jeroen van Hoof
- Version: 0.1

## Browser support
If not mentioned otherwise, the following browsers need to be supported:

| Browser            | Version       |
| -------------------|---------------|
| Firefox            | latest stable |
| Chrome             | latest stable |
| Opera              | latest stable |
| Edge               | latest stable |
| Safari             | latest stable |
| iOS Safari         | latest stable |
| Chrome for android | latest stable |
| Android Browser    | 4.4+          |
| Internet Explorer  | 11            |

Use [caniuse](//caniuse.com) to check whether a certain property is supported by all browsers.

## CSS ruleset
### Syntax
- Each selector on a new line.
- The opening brace (`{`) spaced from the last selector by a single space.
- Each declaration on its own new line.
- A space after the colon (`:`).
- A trailing semi-colon (`;`) at the end of all declarations.
- The closing brace (`}`) on its own new line.
- A new line after the closing brace `}`.
- Two (2) space indents, no tabs.
- Max 80 character wide columns.
- Add a space after commas in values (e.g. `hsla(180, 20%, 20%, 1)`).
- Include a semi-colon at the end of the last declaration in a declaration block.

### Conventions
- Individual words within names are separated by a hyphen (`-`).
- Use `hsla` (hue, saturation, lightness and alpha) for colors, because it makes more intuitive sense what changing the values will do to the color and like rgba, it has an alpha channel. Using the same color definition for all colors, makes the CSS consistent.
- Do not use units after 0 values unless they are required.
- Use shorthand properties where possible.
- Use single quotation marks for attribute selectors and property values.
- Use unit-less line-height.

### Best practices
- Do not use id's (`#`) for styling, because this leads to specificity problems, un-reusable code and furthermore, id's refer to sections in the page.
- Do not use a class in combination with a html component (e.g. `div.element`).
- Limit nesting as much as possible. Do not style entire pages, only reusable components. Especially avoid matching the HTML's entire page structure in your CSS, as this couples HTML and CSS much too tightly and makes the CSS not reusable. You're not building pages, but you're building systems of components.
- Don't use too may font-size declarations. A site is typically made up of a finite number of font treatments, including font size. If you have 10 or more font sizes specified, you probably want to refactor into a standard set of font size classes that can be used in markup.
- Don't use `!important`. Using `!important` overrides any cascaded rule and may lead to specificity war.
- Avoid duplicate properties. When you include the same property twice, it may be intentional (to provide a fallback) or unintentional (copy-paste error). If duplicate properties are placed one after the other with different values, this is okay.
- Avoid undoing CSS (i.e. writing more CSS to undo previous CSS) where possible. Any declarations like these: `border: none;`, `padding: 0;`, `float: none;`, `margin: 0;` are typically bad news. If you are having to remove padding, float, margins, etc., you probably applied them too early.
- Avoid dangerous selectors, i.e. one with far too broad a reach. Examples are styling `div`, setting a `max-width: 650px` on the `body`, or even styling the `header`. The header element does not mean ‘your site’s main header’. And a `max-width` on the body gives you less control: if, for example, you want to add a full-width header and footer later, you need to remove the `max-width` on the `body`.
- Use `box-sizing: border-box`, as this results in cleaner CSS.
- Letting the browser determine the font-size is a good accessibility feature. But setting the base font yourself makes sure you always have the same dimensions and a consistent font across any browser. This makes development and maintenance much easier.
- Avoid sub-pixels (e.g. `0.5rem` with a font-size of `15px` will result in `7.5px`). These are uncontrollable rounded up and down, depending on the order and position of an element. This is especially important for offsets.
- Use rem if you don't need em. But watch out: IE has a problem where it can not use rem in the shorthand version of `font`.

### Order of properties (Optional)
The order of properties is divided into two main categories: _contained properties_ and _propagating properties_. Propagating properties are properties that are inherited by their children, while contained properties do not have any effect on the children. I made up these terms by the way.

__A: contained properties__

0. _position:_ `display` `position` `z-index` `top` `right` `bottom` `left` `float` `clear`
0. _box-model:_ `outline` `width` `height` `min-width` `max-width` `min-height` `max-height` `margin` `border` `padding`
0. _visuals:_  `overflow` `background` `opacity`
0. _miscellaneous:_ all other contained properties

__B: propagating properties__

0. _font:_ `font-family` `font-size` `font-style` `font-variant` `font-weight` `font-size-adjust` `font-stretch` `font` `letter-spacing` `color`
0. _text:_ `direction` `line-height` `orphans` `text-align` `text-align-last` `text-indent` `text-justify` `text-shadow` `text-transform` `quotes` `white-space` `widows` `word-break` `word-spacing` `word-wrap`
0. _visual:_ `cursor` `visibility`
0. _tables:_ `border-collapse` `border-spacing` `caption-side` `empty-cells`
0. _lists:_ `list-style-image` `list-style-position` `list-style-type` `list-style`
0. _tabs:_ `tab-size`


## SASS rules
### Syntax
- Use `@charset 'utf-8';` at the top of the main file, to avoid issues with character encoding.
- Strings should always be wrapped with double quotes (`""`). Quotations are not not required by SASS, but there are several reasons to still use them:
  - Color names are treated as colors when unquoted, which can lead to issues.
  - Most syntax highlighters will choke on unquoted Strings
  - It helps general readability

### Best practices
- Avoid magic numbers (a random number that happens to just work yet is not tied to any logical explanation) and hard-coded/absolute values.
- Do not put other files within your CSS/SASS directory, since you want to decouple CSS from everything else.
- Do not use @mixin's, as they just duplicate code. Use @extend, but only with [selector placeholders](http://www.sitepoint.com/sass-reference/placeholders/), not actual selectors.
- Use ` mix(white, $color, $percentage)` instead of `lighten($color, $percentage)` and ` mix(black, $color, $percentage)` instead of `darken($color, $percentage)`. The benefit of using mix rather than lighten or darken is that it will progressively go to black (or white) as you decrease the proportion of the color, whereas darken and lighten will quickly blow out a color all the way to black or white.
- Do not use variables with color-names (e.g. `$red` or `$blue`) directly in selector declarations. You want to know where they are used. A good way is for example: `$header-color: $blue;`. This specifies that the color is used for the header, and you can easily give it a different color. Brand specific colors are allowed to be named, e.g. `$company-bue`.
- Media queries should be placed inside classes rather than outside.

## Modifiers, states, context, scaffolding
This section is partly based on [MVCSS](http://mvcss.io/styleguide/naming/) and the BEM convention.
### Modifiers
A modifier is an entity that defines the appearance and behavior of a component. After defining the base properties of a component or structure, modifiers exist to allow stylistic tweaks that build on the initial definition. These tweaks are denoted with two hyphens `--`. A button `.button`, for instance, might have a number of different colors and sizes: `.button--small`, `.button--submit`, `.button--cancel`. Define modifiers by their function (`cancel`, `submit`) rather than their appearance (`red`, `blue`).

After creation, elements that need a modifier will use the root class (.btn) and any number of modifiers deemed necessary: `<button class="button button--primary button--large">A Button</button>`.

__Rationale:__ This naming convention avoids cascading and specificity problems, and instantly shows what is modified.

__Pitfall:__ Some people let a modifier extend the block itself, so that in HTML you only need to write `<button class="button--primary">`. This is not the recommended way, because if you need more than one modifier, the modifier that comes later in CSS will overwrite the rules of the modifier that comes first. If you have for example `<button class="button--large button--primary">`, with
```
.button {font-size: 10px}
.button--large {@extend .button; font-size: 15px}
.button--primary {@extend .button; background-color: blue}
```
Then the font-size of 15px will be overwritten by the `.button--primary` class. Furthermore `@extend` is only recommended on placeholder selectors as mentioned in the best practices under the __SASS rules__ section.

__Shorthand notation (optional):__ You can use shorthand notation for sizing and hierarchy modifiers. Here are the shorthand notation modifier conventions that we use for sizing and hierarchy:

```
// Sizing
--xs (extra small)
--s  (small)
--m  (medium)
--l  (large)
--xl (extra large)

// Hierarchy
--a (primary)
--b (secondary)
--c (tertiary)
```

### States
Generally added via JavaScript, states are similar to modifiers but carry conditional context.
`is-` denotes a state, such as `is-active`, and they’re utilized as such:
```
.btn
.btn.is-active {}
```
Never style these classes directly; they should always be used as an adjoining class. This means that the same state names can be used in multiple contexts, but every component must define its own styles for the state (as they are scoped to the component).

### Context
Modularizing styles into self-contained units works well most of the time, but you’ll occasionally need a parent element to fall in line. The most common case tends to be positioning context. If you have a dropdown structure that’s being positioned absolutely, the parent element should be (at least) positioned relatively:
```
.dropdown {}
.has-dropdown {
  position: relative;
}
```
Similar to `is-` with states, classes prefixed with `has-` denote a context selector.

### Scaffolding
Elements nested within a component or structure that need styling based on being there can be added to the scaffolding.
For items in scaffolding, the component/structure name comes first, followed by `__` and the subcomponent/substructure name:
```
.menu {}
.menu__item {}
```
Note that `.menu__item__subitem {}` is not allowed.

## Organization of selectors
Each piece of CSS needs a knowledge of what came before it and what might come after it – a.k.a. dependencies. Poor source order coupled with inherited/inheriting styles can lead to a lot of waste and/or redundancy. A pitfall here is undoing CSS: Writing more CSS in order to undo other CSS.

Often, if you graph a stylesheet by specificity (a specificty graph), it will look like this:
![](stylesheet_mess.png)

[ITCSS](http://csswizardry.net/talks/2014/11/itcss-dafed.pdf) (Inverted Triangle CSS) proposes a model where everything is ordered according to specificity. The goal of this is to help you tame and manage source order and the cascade and reduce waste and redundancy. This will help to avoid specificity problems and undoing CSS.
![](stylesheet_location.png)

However, ITCSS is a framework, so this lacks a clear implementation and rules of what kind of selectors are allowed within these layers, and simply sorting everything according to specificity order makes maintenance harder. So, in order to use the concept in practice, we need to clearly define different types of selectors.

### Allowed selectors
Selectors are allowed if:

1. The selector does not contain an id selector (e.g. `#foo` is not allowed)
2. Nesting does not go deeper than 3 levels (e.g. `.foo .bar .item .key` is not allowed)
3. Elements may not be chained with classes (e.g. `a.foo` or `.foo a.bar` are not allowed)

Usage:
- Selectors need to be allowed if they occur in the CSS.

### Basic selectors
Selectors are basic if:

1. The selector is allowed
2. The selector contains no classes (e.g. `a .foo` is not allowed)
3. The selector contains at most one pseudo element (e.g. `body::before a::after` is not allowed)
4. The selector contains at most two pseudo classes (e.g. `body:focus input:focus:hover` is not allowed)
5. The selector contains at most one universal selector (e.g. `body * a *` is not allowed)

Usage:
- Base classes should be basic
- Basic selectors should be at the beginning of the CSS source.

Examples:
```
a {}
a:hover {}
input:focus:hover {}
*::after {}
```

### Block selectors
Selectors are block selectors if:

1. The selector is allowed
2. The selector contains no (pseudo-elements)
3. The selector contains no universal selectors
4. The selector is not nested
5. If the selector may not be chained, unless it is a state or context element (e.g. `.foo.bar` is not allowed, but `.foo.is-active` or `.foo.has-sidebar` is allowed). An unchained selector is called an __atomic block__, the chained blocks are respectively called __state block__ and __context block__.

Usage:
- Blocks are portable, reusable components
- Utility classes should be atomic blocks
- Block selectors should be after the basic selectors in the CSS source.

Examples:
```
.block {}
.block__item {}
.block--large {}
.block.is-active {}
.block.has-sidebar {}
```

### Structure selectors
Selectors are structure selectors if:

1. The selector is allowed
2. The selector is not a block selector
3. The nesting of the selector is at most two levels deep
4. The selector contains no elements
4. The selector contains no chains

Usage:
- Structures depend on already existing blocks. For example, if you have a navigation with elements inside that must be vertical on mobile, but horizontal on desktop, you can specify this behavior in a structure.
- Use a structure if the selector depends on one or more already existing blocks
- Structure selectors should be after the block selectors in the CSS source

Examples:
```
.block .foo {}
```

### Complex selectors
Selectors are complex if:

1. The selector is allowed
2. The selector is not a block selector
3. The selector is not a structure selector

Usage:
- Complex selectors should be the least occurring selectors in the CSS. Use this if a block or structure does not suffice.
- Complex selectors should be at the end of the CSS source

Examples:
```
.block .foo .bar {}
.block ul li {}
.block a:hover {}
```

## File structure
### Structure

This is an example file structure you could use. Directories or prefixes can be omitted.
```
global/
  _set.functions       - SASS functions to use elsewhere
  _set.settings        - settings, configuration, variables
base/
  _basic.reset         - a reset or normalize file
  _basic.base          - a base file exclusively for basic selectors
util/
  _util.utility        - atomic block selectors, utility classes
blocks/                - block selectors
  _block.componentA
  _block.componentB
structure/             - structure selectors
  _struct.structureA
  _struct.structureB
complex/               - complex selectors
  _complex.complexA
  _complex.complexB
override/              - Overrides
  _override.overrideA
  _override.overrideB
main                   - Main file that loads other files
```
Global files contain settings and functions. Override contains code to override third party HTML, that is for example inserted via JavaScript. These are the only files that do not necessarily need to contain __allowed selectors__. Optionally, use a [Shame CSS](http://csswizardry.com/2013/04/shame-css/) file to keep bad code out of the main files.


### Main file
The main file should be the only Sass file from the whole code base not to begin with an underscore. This file should not contain anything but `@import`'s and comments. The main file itself should have the following syntax:
-  One `@import` per directory/layer
-  A line break after `@import`
-  Each file on its own line
-  A new line after the last import from a directory/layer
-  File extensions and leading underscores omitted

Since the main file controls the source order, we need to import the files in the following order:

1. Settings and functions files
2. Basic files
3. Utility files
4. Block files
5. Structure files
6. Complex files
7. Override files
