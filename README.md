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
* Be specific on the right-hand side of your selector, and less specific on the left.

* ```js
  // Unoptimized
  $("div.data .gonzalez");

  // Optimized
  $(".data td.gonzalez");
  ```
* Avoid Excessive Specificity.

* ```js
  $(".data table.attendees td.gonzalez");

  // Better: Drop the middle if possible.
  $(".data td.gonzalez");
  ```
* Give your Selectors a Context.

* ```js
  // SLOWER because it has to traverse the whole DOM for .class
  $('.class');

  // FASTER because now it only looks under class-container.
  $('.class', '#class-container');
  ```
* Avoid Universal Selectors.

* ```js
  $('div.container > *'); // BAD
  $('div.container').children(); // BETTER
  ```
* Avoid Implied Universal Selectors. When you leave off the selector, the universal selector \`\*\` is still implied.

* ```js
  $('div.someclass :radio'); // BAD
  $('div.someclass input:radio'); // GOOD
  ```
* Don’t Descend Multiple IDs or nest when selecting an ID. ID-only selections are handled using document.getElementById\(\) so don't mix them with other selectors.

* ```js
  $('#outer #inner'); // BAD
  $('div#inner'); // BAD
  $('.outer-container #inner'); // BAD
  $('#inner'); // GOOD, only calls document.getElementById()
  ```

# DOM Manipulation

* Always detach any existing element before manipulation and attach it back after manipulating it.
* ```js
  var $myList = $("#list-container > ul").detach();
  //...a lot of complicated things on $myList
  $myList.appendTo("#list-container");
  ```
* Use string concatenation or array.join\(\) over .append\(\).

* ```js
  // BAD
  var $myList = $("#list");
  for(var i = 0; i < 10000; i++){
      $myList.append("<li>"+i+"</li>");
  }

  // GOOD
  var $myList = $("#list");
  var list = "";
  for(var i = 0; i < 10000; i++){
      list += "<li>"+i+"</li>";
  }
  $myList.html(list);

  // EVEN FASTER
  var array = [];
  for(var i = 0; i < 10000; i++){
      array[i] = "<li>"+i+"</li>";
  }
  $myList.html(array.join(''));
  ```
* Don’t Act on Absent Elements.

* ```js
  // BAD: This runs three functions before it realizes there's nothing in the selection
  $("#nosuchthing").slideUp();

  // GOOD
  var $mySelection = $("#nosuchthing");
  if ($mySelection.length) {
      $mySelection.slideUp();
  }
  ```

# Events

* Use only one Document Ready handler per page. It makes it easier to debug and keep track of the behavior flow.
* DO NOT use anonymous functions to attach events. Anonymous functions are difficult to debug, maintain, test, or reuse.
* ```js
  $("#myLink").on("click", function(){...}); // BAD

  // GOOD
  function myLinkClickHandler(){...}
  $("#myLink").on("click", myLinkClickHandler);
  ```
* Document ready event handler should not be an anonymous function. Once again, anonymous functions are difficult to debug, maintain, test, or reuse.

* ```js
  $(function(){ ... }); // BAD: You can never reuse or write a test for this function.

  // GOOD
  $(initPage); // or $(document).ready(initPage);
  function initPage(){
      // Page load event where you can initialize values and call other initializers.
  }
  ```
* DO NOT use behavioral markup in HTML \(JavaScript inlining\), these are debugging nightmares. Always bind events with jQuery to be consistent so it's easier to attach and remove events dynamically.

* ```php
  <a id="myLink" href="#" onclick="myEventHandler();">my link</a> <!-- BAD -->
  ```



