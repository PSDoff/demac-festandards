# Frontend Guidelines

## HTML

### Semantics

HTML5 provides us with lots of semantic elements aimed to describe precisely the content. Make sure you benefit from its rich vocabulary.

```html
<!-- bad -->
<div id="main">
  <div class="article">
    <div class="header">
      <h1>Blog post</h1>
      <p>Published: <span>21st Feb, 2015</span></p>
    </div>
    <p>…</p>
  </div>
</div>

<!-- good -->
<main>
  <article>
    <header>
      <h1>Blog post</h1>
      <p>Published: <time datetime="2015-02-21">21st Feb, 2015</time></p>
    </header>
    <p>…</p>
  </article>
</main>
```

Make sure you understand the semantics of the elements you're using. It's worse to use a semantic  
element in a wrong way than staying neutral.

```html
<!-- bad -->
<h1>
  <figure>
    <img alt=Company src=logo.png>
  </figure>
</h1>

<!-- good -->
<h1>
  <img alt=Company src=logo.png>
</h1>
```

_NOTE: HTML5 may not be fully supported by our browser support matrix.  
_[_HTML5 Shiv_](https://github.com/aFarkas/html5shiv)_ can resolve issues in browsers that don't support by creating the required recognition of common HTML5 elements._

### Brevity

Keep your code terse. Forget about your old XHTML habits.

```html
<!-- bad -->
<!doctype html>
<html lang=en>
  <head>
    <meta http-equiv=Content-Type content="text/html; charset=utf-8" />
    <title>Contact</title>
    <link rel=stylesheet href=style.css type=text/css />
  </head>
  <body>
    <h1>Contact me</h1>
    <label>
      Email address:
      <input type=email placeholder=you@email.com required=required />
    </label>
    <script src=main.js type=text/javascript></script>
  </body>
</html>

<!-- good -->
<!doctype html>
<html lang=en>
  <meta charset=utf-8>
  <title>Contact</title>
  <link rel=stylesheet href=style.css>

  <h1>Contact me</h1>
  <label>
    Email address:
    <input type=email placeholder=you@email.com required>
  </label>
  <script src=main.js></script>
</html>
```

### Accessibility

Accessibility shouldn't be an afterthought. You don't have to be a WCAG expert to improve your  
website. You can start immediately by fixing the little things that make a huge difference, such as:

* learning to use the `alt` attribute properly
* making sure your links and buttons are marked as such \(no `<div class="button">` atrocities\)
* not relying exclusively on colors to communicate information
* explicitly labelling form controls

```html
<!-- bad -->
<h1><img alt="Logo" src="logo.png"></h1>

<!-- good -->
<h1><img alt="My Company, Inc." src="logo.png"></h1>
```

### Language

While defining the language and character encoding is optional, it's recommended to always declare  
both at document level, even if they're specified in your HTTP headers. Favor UTF-8 over any other  
character encoding.

```html
<!-- bad -->
<!doctype html>
<title>Hello, world.</title>

<!-- good -->
<!doctype html>
<html lang=en>
  <meta charset=utf-8>
  <title>Hello, world.</title>
</html>
```

### Performance

Unless there's a valid reason for loading your scripts before your content, don't block the  
rendering of your page. If your style sheet is heavy, isolate the styles that are absolutely  
required initially and defer the loading of the secondary declarations in a separate style sheet.  
Two HTTP requests is significantly slower than one, but the perception of speed is the most  
important factor.

```html
<!-- bad -->
<!doctype html>
<meta charset=utf-8>
<script src=analytics.js></script>
<title>Hello, world.</title>
<p>...</p>

<!-- good -->
<!doctype html>
<meta charset=utf-8>
<title>Hello, world.</title>
<p>...</p>
<script src=analytics.js></script>
```

## CSS

### Semicolons

While the semicolon is technically a separator in CSS, always treat it as a terminator.

```css
/* bad */
div {
  color: red
}

/* good */
div {
  color: red;
}
```

### Box model

The box model should ideally be the same for the entire document. A global  
`* { box-sizing: border-box; }` is fine, but don't change the default box model  
on specific elements if you can avoid it.

```css
/* bad */
div {
  width: 100%;
  padding: 10px;
  box-sizing: border-box;
}

/* good */
* {
    box-sizing: border-box;
}

div {
  padding: 10px;
}
```

### Flow

Don't change the default behavior of an element if you can avoid it. Keep elements in the  
natural document flow as much as you can. For example, removing the white-space below an  
image shouldn't make you change its default display:

```css
/* bad */
img {
  display: block;
}

/* good */
img {
  vertical-align: middle;
}
```

Similarly, don't take an element off the flow if you can avoid it. The more elements  
that are removed from the document flow, the less context there is for the site  
to respond in an intelligent fashion.

```css
/* bad */
div {
  width: 100px;
  position: absolute;
  right: 0;
}

/* good */
div {
  width: 100px;
  margin-left: auto;
}
```

### Positioning

There are many ways to position elements in CSS but try to restrict yourself to the  
properties/values below. By order of preference:

```
display: block;
display: flex;
position: relative;
position: sticky;
position: absolute;
position: fixed;
```

### Selectors

Minimize selectors tightly coupled to the DOM. Consider adding a class to the elements  
you want to match when your selector exceeds 3 structural pseudo-classes, descendant or  
sibling combinators.

```css
/* bad */
div:first-of-type :last-child > p ~ * {}

/* good */
div:first-of-type .info {}
```

### Specificity

Don't make values and selectors hard to override. Minimize the use of `id`'s  
and avoid `!important`.

```css
/* bad */
.bar {
  color: green !important;
}
.foo {
  color: red;
}

/* good */
.foo.bar {
  color: green;
}
.foo {
  color: red;
}
```

### Overriding

Overriding styles makes selectors and debugging harder. Avoid it when possible.

```css
/* bad */
li {
  visibility: hidden;
}
li:first-child {
  visibility: visible;
}

/* good */
li + li {
  visibility: hidden;
}
```

### Inheritance

Don't duplicate style declarations that can be inherited.

```css
/* bad */
div h1, div p {
  text-shadow: 0 1px 0 #fff;
}

/* good */
div {
  text-shadow: 0 1px 0 #fff;
}
```

### Brevity

Keep your code terse. Use shorthand properties and avoid using multiple properties when  
it's not needed.

```css
/* bad */
div {
  transition: all 1s;
  top: 50%;
  margin-top: -10px;
  padding-top: 5px;
  padding-right: 10px;
  padding-bottom: 20px;
  padding-left: 10px;
}

/* good */
div {
  transition: 1s;
  top: calc(50% - 10px);
  padding: 5px 10px 20px;
}
```

### Vendor prefixes

Kill obsolete vendor prefixes aggressively. If you need to use them, insert them before the  
standard property.

```css
/* bad */
div {
  transform: scale(2);
  -webkit-transform: scale(2);
  -moz-transform: scale(2);
  -ms-transform: scale(2);
  transition: 1s;
  -webkit-transition: 1s;
  -moz-transition: 1s;
  -ms-transition: 1s;
}

/* good */
div {
  -webkit-transform: scale(2);
  transform: scale(2);
  transition: 1s;
}
```

_NOTE: Existing task runners at Demac should be auto-prefixing for you  
based on data from _[_caniuse.com_](http://caniuse.com/)_. Avoid writing your own vendor prefixes._

### Animations

Favor transitions over animations. Avoid animating other properties than  
`opacity` and `transform`.

```css
/* bad */
div:hover {
  animation: move 1s forwards;
}
@keyframes move {
  100% {
    margin-left: 100px;
  }
}

/* good */
div:hover {
  transition: 1s;
  transform: translateX(100px);
}
```

### Units

Use unit-less values when you can. Favor `rem` if you use relative units. Prefer seconds over  
milliseconds.

```css
/* bad */
div {
  margin: 0px;
  font-size: .9em;
  line-height: 22px;
  transition: 500ms;
}

/* good */
div {
  margin: 0;
  font-size: .9rem;
  line-height: 1.5;
  transition: .5s;
}
```

### Colors

If you need transparency, use `rgba`. Otherwise, always use the hexadecimal format.  
Prefer long format hex values.

```css
/* bad */
div {
  color: hsl(103, 54%, 43%);
}

/* good */
div {
  color: #5a3;
}

/* best */
div {
    color: #55aa33;
}
```

### Drawing

Avoid HTTP requests when the resources are easily replicable with CSS.

```css
/* bad */
div::before {
  content: url(white-circle.svg);
}

/* good */
div::before {
  content: "";
  display: block;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #fff;
}
```

### Hacks

Don't use them.

```css
/* bad */
div {
  // position: relative;
  transform: translateZ(0);
}

/* good */
div {
  /* position: relative; */
  will-change: transform;
}
```



