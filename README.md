# jQuery Code Style \| jQuery Best Practices

## Loading jQuery

* Always try to use a CDN to include jQuery on your page.
* ```js
  <script type="text/javascript" src="http://ajax.microsoft.com/ajax/jquery/jquery-1.4.2.min.js"></script>
  <script type="text/javascript">
  if (typeof jQuery == 'undefined') {
      document.write(unescape("%3Cscript src='/js/jquery-1.4.2.min.js' type='text/javascript'%3E%3C/script%3E"));
  }
  </script>
  ```
* Use protocol-relative/protocol-independent  URL \(leave http: or https: out\) as shown above.

* If possible, keep all your JavaScript and jQuery includes at the bottom of your page.
* What version to use?
  * DO NOT use jQuery version 2.x if you support Internet Explorer 6/7/8.
  * For new web-apps, if you do not have any plugin compatibility issue, it's highly recommended to use the latest jQuery version.
  * When loading jQuery from CDN's, always specify the complete version number you want to load \(Example: 1.11.0 as opposed to 1.11 or just 1\).
  * DO NOT load multiple jQuery versions.
  * DO NOT use jquery-latest.js from jQuery CDN.
* If you are using other libraries like Prototype, MooTools, Zepto etc. that uses $ sign as well, try not to use $ for calling jQuery functions and instead use jQuery simply. You can return control of $ back to the other library with a call to $.noConflict\(\).
* For advanced browser feature detection, use [Modernizr](https://modernizr.com/).

## jQuery Variables

* All variables that are used to store/cache jQuery objects should have a name prefixed with a $.
* Always cache your jQuery selector returned objects in variables for reuse.
* ```js
  var $myDiv = $("#myDiv");
  $myDiv.click(function(){...});
  ```
* Use camel case for naming variables.

# Selectors

* Use ID selector whenever possible \(but only if it makes sense to do so\). It is faster because they are handled using document.getElementById\(\).
* When using class selectors, don't use the element type in your selector.
* ```js
  var $products = $("div.products"); // SLOW
  var $products = $(".products"); // FAST
  ```
* Use find for Id-&gt;Child nested selectors. The .find\(\) approach is faster because the first selection is handled without going through the Sizzle selector engine.

* ```js
  // BAD, a nested query for Sizzle selector engine
  var $productIds = $("#products div.id");

  // GOOD, #products is already selected by document.getElementById() so only div.id needs to go through Sizzle selector engine
  var $productIds = $("#products").find("div.id");
  ```



