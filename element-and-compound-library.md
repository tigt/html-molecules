# Element & Compound Library

This is a list and display of all the HTML elements and common combinations of elements (compounds, [thanks to tantek for the name](http://tantek.com/presentations/2005/09/elements-of-xhtml/)) to test stylesheets on.

## Text-level semantics

### `<em>` & `<strong>`

Mr. Grinch, the words that best describe you are as follows, and I quote: *Stink,* **stank,** ***stunk!***

I've seen people style these in ways other than just bolding and italicizing, but it always look pretty weird. The only case I can see that being useful is for non-Roman alphabets, which weren't designed for either method of text emphasis.

### `<abbr>`

This is an abbreviation with an expansion: <abbr title="World Health Organization">WHO</abbr>.

This is an abbreviation without an expansion: <abbr>W3C</abbr>

Suggested CSS would be one of the few supported CSS Speech properties, `speak`:

```css
abbr {
  speak: spell-out;
}

abbr[title] {
  border-bottom: 1px dotted gray;
  cursor: help;
}

@supports (text-decoration: dotted underline) {
  abbr[title] {
    text-decoration: dotted underline;
    border: none;
  }
}
```

(Latest versions of Firefox changed the UA stylesheet to include `text-decoration: dotted underline` so.)

I personally avoid relying upon `title`, since it doesn't work for non-mouse users. With the rise of touchscreens, this includes nearly everyone, so. I tend to code like this:

```html
<p>
When <abbr>NATO <dfn>(North American Trade Organization)</dfn></abbr> last convened...
</p>
```

### `<a>`

Heydon Pickering made an excellent stylesheet utility for indicating links, [Auticons](http://heydonworks.com/auticons-icon-font/).

#### Without `href`

I don't recommend doing so, since it has accessibility problems (assistive tech valiantly attempts to guess that JavaScript is going to do something to a `href`less anchor and announces it as one hoping you'll do the right thing), but if you're going to go ahead:

```css
a:not([href]) {
  color: inherit;
  text-decoration: none;
}
```

The one use-case I can think of for this is in navigation, when you're on the page that it would normally link to:

```html
<nav>
<a href="/about">About</a>
<a>Contact</a>
<a href="/links">Links</a>
</nav>
```

#### Block-level

#### Pseudo-classes

#### `rel` values

```css
a[rel~="tag"]::before {
  content: "#";
}
```

(The space-separated list attribute selector was practically *made* for `rel`.)

>For a and area elements, on some browsers, the help keyword causes the link to use a different cursor.

#### Particular kinds of links

##### `mailto:`

It is considerate to warn the user when a `mailto:` link is used, since it may fire up their default email client.

##### File formats

PDF and Word documents are especially annoying to mobile users when triggering large downloads. If you inform the user what's behind the link, they can choose to save their data plans.

```css
a[href$="pdf" i]::after {
  content: "[PDF]";
  font-size: small;
  color: red;
}
```

#### `target`

A small icon indicating the link will open in a new tab/window is a good warning.

#### `type` and `hreflang`

The only way I see these ever being used is a CMS that will auto-fetch links, note the `Content-Type` and `Content-Language` headers, and then append them.

#### `download`

```css
a[download]::after {
  content: "↓";
}
```

`download` isn't just a boolean attribute; you can also include a suggested filename:

```html
<a href="/report.pdf" download="Quarterly Earnings Report.pdf">Quarterly Earnings Report</a>
```

### `<span>`

### `<ruby>`, `<rb>` `<rt>`, `<rp>`, and `<rtc>`

These semantics are from CJK languages, and so uses may not be obvious for us foreigners. [HTMLDoctor valiantly attempted using `<ruby>` in English](http://html5doctor.com/ruby-rt-rp-element/):

>

#### Dictionary entries with `<dl>`, `<ruby>`, and `<dfn>`

However, if you are managing a multilingual site, getting `<ruby>` right is critical.

#### Nested `<ruby>`

### `<dfn>`

By default in most browsers, `<dfn>` is rendered in italics. I personally prefer boldface, if only because that's how all my textbooks have highlighted their key terms. Most of them actually did this:

```css
dfn {
  font-weight: bolder;
  background-color: yellow;
}
```

#### `<dfn title>`

>If the title attribute of the dfn element is present, then it must contain only the term being defined.

### `<mark>`

Used especially for search results, emphasing sections in quotes, or code examples.

#### Block-level usage

### `<sub>` and `<sup>`

#### Footnotes with `<sup><a>`

#### Nesting

Remember that technically super and subscripts can be nested, and your styling should respect that.

### `<i>`, `<u>`, & `<b>`

They have been given nod to their general-purpose of delineating text that means something different. It's recommended to use a class name more clearly indicating why the text was delineated, such as `narration`, `ship-name`, etc.

#### `<i lang>`

#### Article ledes: `<article><p><b>Lede text...</b></p></article>

#### Chinese proper names with `<u>`

#### Keyboard shortcuts

Interestingly, Internet Explorer used to revitalize `<u>` for keyboard shortcut indicators with its own proprietary CSS and the `accesskey` attribute:

```css
u {
  -ms-accelerator: true;
}
```

```html
<samp><kbd><u>F</u>ind...</kbd></samp>

<input type="text"
    accesskey="F"
    value="Enter a term to find">
```

### `<s>`

### `<bdi>` and `<bdo>`

I've come to use `<bdi>` as a way of indicating "user-generated content here," since that's its whole point for existence.

#### `dir`

### `<small>`

### `<q>`

#### `cite` attribute

Unfortunately there isn't a very useful way for readers to use `<q cite>` without getting JavaScript involved.

#### Nested quotes

If you're implementing your own quotation mark styles, be sure to account for nested quotes! Since quotes can theoretically be nested infinitely, the CSS `quotes` property has a built-in mechanism for alternating quotation marks.

### `<cite>`

The oldest usage of `<cite>` is for titles of works. It commonly wraps a hyperlink to that work. In some cases, it also wraps the spelled-out URL of a source, such as permalinks or URLs displayed in search engine results.

It can also work as a person who acts as the source of a quotation.

### `<ins>` and `<del>`

Block-level or text-level

####`datetime` and `cite` attributes

#### Difficulties with lists and tables

### `<var>`

I like to always put `var` in italic serifs, unless referring to code. (Which would be `<code><var>`, so.)

#### `<var><sub>`

Since variables representing different instances of the same concept are typically represented using the same letter but with a subscript, the spec specifically calls out `<sub>` nested inside `<var>` being ideal for the purpose.

### `<code>`

I've diverged a bit from the standards and personally use the `type` attribute on `<code>` to indicate what it contains, just like `<script>` and `<style>`

### `<kbd>` and `<samp>`

These two have unusually specific semantics when nested into compounds with each other. These advanced usages aren't mandatory, but I tend to find them useful enough to bother.

#### `<kbd>` inside `<samp>`

#### `<samp>` inside `<kbd>`

#### `<kbd>` inside `<kbd>`

Nested `<kbd>` represents the actual keys used in computer input, such as displaying key combination commands:

```html
To print, press <kbd><kbd>Ctrl</kbd> + <kbd>P</kbd></kbd>
```

The CSS for this is simple enough:

```css
kbd {font-family: monospace}

kbd > kbd {
  background-color: grey;
  border: 1px darkgrey outset;
  border-radius: .1em;
}
```

### `<br>` and `<wbr>`

Semantic usage of `<br>` would be the lines of an address, the lines of a poem or song, and indicating hard wraps in code.

It is rare that you would want to explicitly display these two except for diagnostic purposes. (Indicating code wrapping?)

#### `<p><br></p>`

### `<data>`

The `value` attribute is required, since without it there's no point using `data`.

You can surface is using something like this:

```css
data[value] {position: relative}

data[value]:matches(:hover, :focus)::after {
  content: attr(value);
  position: absolute;
  right: 0;
}
```

### `<time>`

With the rise of OpenType features, it's a smart idea to enable figures that work especially well for time display.

#### `datetime`

Oddly enough, this isn't a required attribute.

I personally find value in displaying the `datetime` attribute, since it has, despite being designed otherwise, become a sort of temporal lingua franca, having flouted *everyone's* time notation and therefore becoming a compromise.

## Grouping semantics

### `<p>`

### `<div>`

### `<figure>` and `<figcaption>`

There are many things that can go into a `<figure>`: Images, videos, tables, quotes, inline SVG or MathML...

### `<hr>`

#### `aria-orientation`

```css
hr[aria-orientation="vertical"] {
  transform: rotate(90deg);
}
```

### `<pre>`

#### ASCII art

I provide alternate text for any ASCII art using `aria-label`, and reveal it like so:

```css
pre[aria-label]:hover::before {
  content: attr(aria-label);
  display: block
}
```

#### Concrete poetry

This is an intentional blurring of content and presentation, which makes nerds mad but artists gleeful. Such poems often require certain typefaces as well to ensure proper presentation, so including `font-size-adjust` is practically mandatory.

#### `<pre>` and `<code>`

<pre><code type="text/css">


</code></pre>

#### Displaying a command-line session with `<kbd>` and `<samp>`

### `<ol>`

The reinstated attributes can trip you up if you defined your own custom counters for them:

`reversed`
`start`

### `<ul>`

### `<dl>`

### `<blockquote>`

#### `cite` attribute

#### With a footer

#### `<blockquote><cite>`

## Embedding

### `<img>`

#### Visible `alt` text

Usually there is no need to style alt text when the image fails, as it's designed to work as an alternative, drop-in replacement for the image. But there are situations.

For images that intentionally omit an `alt` value because they're presentational, you could do something like this:

```css
img[alt=""]:-moz-broken {
  display: none;
}
```

#### `generator-unable-to-provide-required-alt`

Yes, this is real. Amazing, right?

#### Responsive CSS and `height` and `width`

Happily, the reflow-preventing attributes work just as you'd hope, with the browser respecting the indicated aspect ratio of the image as it scales it.

### `<map>` and `<area>`

```css
img[usemap],
img[ismap] {
  text-decoration: blink;
}
```

Notice that `ismap` requires a wrapping anchor for fallback (even though all browsers that need it are stone dead), and neither play nicely with responsive images, so this could be used as a diagnostic style:

```css
img[ismap] {
  border: 3px solid red;
}

a img[ismap] {
  border-color: green;
}

img[ismap][srcset],
img[usemap][srcset],
picture > img[ismap],
picture > img[usemap] {
  border: 3px solid red;
}
```

### `<picture>` and `srcset`

`<picture>` isn't really the sort of element you'd style, but it does provide for pseudo-elements whereas `<img>` doesn't.

If you were doing a sweep of what images were responsive and which weren't on a site, you could put this in a diagnostic stylesheet:

```css
img {
  border: 3px solid red;
}

picture > img,
img[srcset] {
  border-color: green;
}
```

You could even show the `<source>` elements if you needed to:

```css
picture > source {
  display: list-item;
}

picture > source::before {
  content: attr(srcset);
}

picture > source::after {
  content: attr(type) + attr(media);
}
```

### `<object>` and `<embed>`

As a result, the only default styles you would want to include would be things like `max-width`.

### `<canvas>`

### `<iframe>`

#### `<iframe seamless>`

This is about as close as we'll get to code imports for HTML.

```css
border: none;
```

### `<audio>`

#### Audio with `<track>`

#### `controls`

`<audio>` elements without `controls` should not be displayed at all:

```css
audio:not([controls]) {
  display: none;
}
```

If you want to make your own audio player, you'll need something like this:

```html
<audio src="/song.mp3" id="audio">
  <a href="/song.mp3">
    Listen to the song
  </a>
</audio>

<menu type="toolbar" aria-controls="audio">
  <li><button>Play</button></li>
  <li><button>Pause</button></li>
</menu>
```

### `<video>`

#### Video with `<track>`

#### `<video controls>`

If you want to implement your own controls, it's similar to how you'd do it for `<audio>`:

```html
<video src="/movie.mp4" id="video">
  <a href="/movie.mp3">
    Watch the movie
  </a>
</video>

<menu type="toolbar" aria-controls="video">
  <li><button>Play</button></li>
  <li><button>Pause</button></li>
</menu>
```

### `<svg>`

Internet Explorer forget to add the suggested standard style of `svg:not(:root) { overflow: hidden; }`, which you really should add if you're not using something like normalize.css.

#### Internal SVG compounds

##### `<text><a>`

The default bright-blue underlines doesn't happen in SVG, and if you use linked SVG text you may need to reinstate your link appearances.

### `<math>`

Especially consider fallback math styles, as only Safari and Firefox implement the display of MathML. I personally like to use a grey background and a serif font.

[There are quite a few OpenType features that should also be enabled for `<math>` fallbacks.](https://www.fontfont.com/staticcontent/downloads/FF_OT_User_Guide.pdf)

## Forms

### `<form>`

### `<fieldset>` and `<legend>`

### `<input>` types

4.10.5.1.1 Hidden state (type=hidden)
4.10.5.1.2 Text (type=text) state and Search state (type=search)
4.10.5.1.3 Telephone state (type=tel)
4.10.5.1.4 URL state (type=url)
4.10.5.1.5 E-mail state (type=email)
4.10.5.1.6 Password state (type=password)
4.10.5.1.7 Date and Time state (type=datetime)
4.10.5.1.8 Date state (type=date)
4.10.5.1.9 Month state (type=month)
4.10.5.1.10 Week state (type=week)
4.10.5.1.11 Time state (type=time)
4.10.5.1.12 Number state (type=number)
4.10.5.1.13 Range state (type=range)
4.10.5.1.14 Colour state (type=color)
4.10.5.1.15 Checkbox state (type=checkbox)
4.10.5.1.16 Radio Button state (type=radio)
4.10.5.1.17 File Upload state (type=file)
4.10.5.1.18 Submit Button state (type=submit)
4.10.5.1.19 Image Button state (type=image)
4.10.5.1.20 Reset Button state (type=reset)
4.10.5.1.21 Button state (type=button)

### `<textarea>`

#### `wrap`



### `<select>`, `<option>`, and `<optgroup>`

### `<button>`

### `<output>`

### `<meter>`

#### `high`

#### `low`

#### `optimal`

### `<progress>`

### `<keygen>` and `<datalist>`

### `placeholder`

### `readonly`

### Validation pseudo-classes and pseudo-elements

### Labels

## Document sectioning

### `<html>` and `<body>`

#### Default font selection using `lang`

#### `dir`

If you're working on a multi-lingual site, be sure to check how things work in the other value of `dir`. Common culprits include:

### `<aside>`

### `<address>`

>The address element represents the contact information for its nearest article or body element ancestor. If that is the body element, then the contact information applies to the document as a whole.

>The address element must not be used to represent arbitrary addresses (e.g. postal addresses), unless those addresses are in fact the relevant contact information. (The p element is the appropriate element for marking up postal addresses in general.)

>The address element must not contain information other than contact information.

This has italics in default stylesheets, which isn't a bad idea. Still, you may want to override that.

It is highly likely that you will use `<a>` inside `<address>`, but that may not warrant custom styling for your case.

It's fairly likely to include `<address>` inside of an `<article>`'s `<header>` or `<footer>`.

### `<h1>`&ndash;`<h6>`

>Certain elements are said to be sectioning roots, including blockquote and td elements. These elements can have their own outlines, but the sections and headings inside these elements do not contribute to the outlines of their ancestors.

>    blockquote body details dialog fieldset figure td

#### Default HTML5 nested sectioning styles

### `<section>`

#### Outside `<article>`

#### Inside `<article>

### `<header>`

### `<article>`

Since it's such a generic element, you presumably won't be styling just its tag, but as an example:

```css
article {
  max-width: 60em; /* ideal line-length for reading */
}
```

#### `<address>` inside `<article>`

#### `<header>` and `<footer>` inside `<article>`

### `<main>`

### `<nav>`

Since this element will *almost certainly* include hyperlinks, you may want to style those slightly differently:

<figure>
  <nav>
    <a href="#">Link 1</a>
    <a href="#">Link 1</a>
    <a href="#">Link 1</a>
  </nav>
</figure>

It's likely that the navigation would include lists, as well:

### `<footer>`

#### Legal text: `<footer><small>`

### `<aside>`

#### Pull quotes: `<aside><q>` or `<aside><blockquote>`

## Tables

### Basic `<table>`

### Accessibility attributes

### `<th>`

### `<col>` & `<colgroup>`

### `<caption>`

## Interactivity

### `<menu>` & `<menuitem>`

### `<details>` & `<summary>`

#### `open`

### `<dialog>`

### Generic use of pseudo-classes, especially `:focus`

### Drag & Drop attributes: `draggable` and `dropzone`

```css
[draggable] {
  cursor: grab;
}

[draggable]:active {
  cursor: grabbing;
}
```

### `accesskey`

### ARIA roles

### `contenteditable`

### `contextmenu`

```css
*[contextmenu] {
  cursor: contextmenu;
}
```