# jQuery Code Style \| jQuery Best Practices

## Loading jQuery {#loading-jquery}

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

## jQuery Variables {#jquery-variables}

* All variables that are used to store/cache jQuery objects should have a name prefixed with a $.
* Always cache your jQuery selector returned objects in variables for reuse.
* ```js
  var $myDiv = $("#myDiv");
  $myDiv.click(function(){...});
  ```
* Use camel case for naming variables.

## Selectors {#selectors}

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

## DOM Manipulation {#dom-manipulation}

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

## Events {#events}

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

  ```js
  $("#myLink").on("click", myEventHandler); // GOOD
  ```
* When possible, use custom namespace for events. It's easier to unbind the exact event that you attached without affecting other events bound to the DOM element.

* ```js
  $("#myLink").on("click.mySpecialClick", myEventHandler); // GOOD
  // Later on, it's easier to unbind just your click event
  $("#myLink").unbind("click.mySpecialClick");
  ```
* Use event delegation when you have to attach same event to multiple elements. Event delegation allows us to attach a single event listener, to a parent element, that will fire for all descendants matching a selector, whether those descendants exist now or are added in the future.

* ```js
  $("#list a").on("click", myClickHandler); // BAD, you are attaching an event to all the links under the list.
  $("#list").on("click", "a", myClickHandler); // GOOD, only one event handler is attached to the parent.
  ```

## Ajax {#ajax}

* Avoid using .getJson\(\) or .get\(\), simply use the $.ajax\(\) as that's what gets called internally.
* DO NOT use http requests on https sites. Prefer schemaless URLs \(leave the protocol http/https out of your URL\).
* DO NOT put request parameters in the URL, send them using data object setting.
* ```js
  // Less readable...
  $.ajax({
      url: "something.php?param1=test1&param2=test2",
      ....
  });

  // More readable...
  $.ajax({
      url: "something.php",
      data: { param1: test1, param2: test2 }
  });
  ```
* Try to specify the dataType setting so it's easier to know what kind of data you are working with. \(See Ajax Template example below\).

* Use Delegated event handlers for attaching events to content loaded using Ajax. Delegated events have the advantage that they can process events from descendant elements that are added to the document at a later time \(example Ajax\).

* ```js
  $("#parent-container").on("click", "a", delegatedClickHandlerForAjax);
  ```
* Use Promise interface: \(EXAMPLES: [http://www.htmlgoodies.com/beyond/javascript/making-promises-with-jquery-deferred.html\](http://www.htmlgoodies.com/beyond/javascript/making-promises-with-jquery-deferred.html\)\)

* ```js
  $.ajax({ ... }).then(successHandler, failureHandler);

  // OR
  var jqxhr = $.ajax({ ... });
  jqxhr.done(successHandler);
  jqxhr.fail(failureHandler);
  ```
* Sample Ajax Template:

* ```js
  var jqxhr = $.ajax({
      url: url,
      type: "GET", // default is GET but you can use other verbs based on your needs.
      cache: true, // default is true, but false for dataType 'script' and 'jsonp', so set it on need basis.
      data: {}, // add your request parameters in the data object.
      dataType: "json", // specify the dataType for future reference
      jsonp: "callback", // only specify this to match the name of callback parameter your API is expecting for JSONP requests.
      statusCode: { // if you want to handle specific error codes, use the status code mapping settings.
          404: handler404,
          500: handler500
      }
  });
  jqxhr.done(successHandler);
  jqxhr.fail(failureHandler);
  ```

## Effects and Animations {#effects-and-animations}

* Adopt a restrained and consistent approach to implementing animation functionality.
* DO NOT over-do the animation effects until driven by the UX requirements.
  * Try to use simeple show/hide, toggle and slideUp/slideDown functionality to toggle elements.

## Plugins {#plugins}

* Always choose a plugin with good support, documentation, testing and community support.
* Check the compatibility of plugin with the version of jQuery that you are using.
* Any common reusable component should be implemented as a jQuery plugin.

## Method Chaining {#method-chaining}

* Use chaining as an alternative to variable caching and multiple selector calls.
* ```js
  $("#myDiv").addClass("error").show();
  ```
* Whenever the chain grows over 3 links or gets complicated because of event assignment, use appropriate line breaks and indentation to make the code readable.

* ```js
  $("#myLink")
      .addClass("bold")
      .on("click", myClickHandler)
      .on("mouseover", myMouseOverHandler)
      .show();
  ```
* For long chains it is acceptable to cache intermediate objects in a variable.

## Miscellaneous {#misc}

* Use Object literals for parameters.
* ```js
  $myLink.attr("href", "#").attr("title", "my link").attr("rel", "external"); // BAD, 3 calls to attr()
  // GOOD, only 1 call to attr()
  $myLink.attr({
      href: "#",
      title: "my link",
      rel: "external"
  });
  ```
* Do not mix CSS with jQuery.

* ```js
  $("#mydiv").css({'color':red, 'font-weight':'bold'}); // BAD
  ```

  ```css
  .error { color: red; font-weight: bold; } /* GOOD */
  ```

  ```js
  $("#mydiv").addClass("error"); // GOOD
  ```
* DO NOT use Deprecated Methods. It is always important to keep an eye on deprecated methods for each new version and try avoid using them. [Click here](http://api.jquery.com/category/deprecated/) for a list of deprecated methods.

* Combine jQuery with native JavaScript when needed.

* ```js
  $("#myId"); // is still a little slower than...
  document.getElementById("myId");
  ```



