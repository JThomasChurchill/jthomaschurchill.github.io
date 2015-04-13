---
layout: post
title:  "JavaScript Modernization"
date:   2015-01-01 10:20:18
categories: JavaScript IE
css_class: "tutorial"
---

## Internet Explorer Upgrade

Microsoft has announced it is dropping support for Internet Explorer (IE) versions 8, 9, and 10 for users running Windows 7 or newer. This change will take place January 12, 2016. Without security patches being supplied by Microsoft, it will no longer be safe to allow IE8, IE9, or IE10 to be installed on your machine.

This change is long overdue, but it presents a challenge to organizations running applications using syntax only supported by these soon-to-be obsolete browsers. This page is intended to give some basic information about common cross-browser compatibility issues between older versions of IE and modern browsers.

The [Mozilla Developer Network (MDN)](https://developer.mozilla.org/en-US/docs/Migrate_apps_from_Internet_Explorer_to_Mozilla) has some usefull advice on converting code away from older IE or Netscape syntax.

There is a lot of syntax specific to Internet Explorer that can be rewritten either in standard JavaScript or in jQuery in order to be cross-browser compatible. Technically Microsoft doesn't support JavaScript, but rather "JScript", which is Internet Explorer's dialect of the [ECMAScript](https://en.wikipedia.org/wiki/ECMAScript) standard that JavaScript is based on.

---

### The Script Tag

Before getting down to the important stuff there is a non-critical style issue I would like to address...

Capital letters for element or attribute names is ugly and very old-fashioned! The modern way to open and close a script tag is with lower-case tag elements, and with the <code>type="text/javascript"</code> attribute. The <code>language="javascript"</code> attribute is obsolete. Some people prefer even to leave out <code>type="text/javascript"</code> since it is the default value assumed by the browser.

This old style:

```html
<SCRIPT LANGUAGE="JavaScript">
    // code...
</SCRIPT>
```

...should be changed to this:

```html
<script type="text/javascript">
    // code...
</script>
```

...or even this, since the value "text/javascript" is already the default

``` html
<script>
    // code...
</script>
```

#### The Finer Details
Templating frameworks like Knockout, as well as other technologies may use a type other than "text/javascript" for script tags. The main thing to remember is that script tags with type="text/javascript" (including tags with no type specified) will be parsed as JavaScript as the page loads. Any other type will be ignored.

One argument for leaving the type out is that generally one should keep things short and sweet to save bandwidth. An argument for including the type is that in HTML 4.01 it is a required attribute, and may therefor throw an error if you validate your code using a tool like the [W3C Markup Validation Service](http://validator.w3.org/check).

You *really should* be validating your markup, and if you are looking for errors on a broken page, little issues like this can add up quickly and bury the one you need to see to fix the page. The W3C validator will not throw an error for a missing type attribute in an HTML5 document.

---

## Finding Elements

One of the compatibility issues that comes up most often is <code>document.all.myElement</code>, which is also sometimes written <code>document.all["myElement"]</code>. This code dates back to the days of IE4 and has been obsolete since 1998 when IE5 was released. Below is the obsolete syntax along with the JavaScript and jQuery equivalents.

```javascript
document.all.myElement // obsolete IE4 syntax
document.all["myElement"] // obsolete IE4 syntax
document.getElementById("myElement") // JavaScript
$("#myElement") // jQuery
```

A less popular option, although I'm not sure why since it's very useful, is <code>document.querySelector</code> and <code>document.querySelectorAll</code>.

```javascript
document.querySelector(".my-class"); // returns first matching element found
document.querySelectorAll(".my-class"); // returns a list* of matching elements
```

The method <code>querySelectorAll</code> returns a [NodeList](//developer.mozilla.org/en-US/docs/Web/API/NodeList) object, which is like an Array in that it has numbered indexes, but don't try to run Array methods on it!

---

## Event Handling

Pretty much everything about events and event handling is different between older IE and modern browsers.

```javascript
// This is obsolete IE syntax
window.attachEvent("onload", function(){
    myFunction();
});
```

```javascript
// this is the jQuery way
$(window).load(function(event) {
    myFunction(event);
});
```

---

## Accessing the Event Object

When an event happens an Event object is created by the browser. In the case of a mouse click, an Event object is created that contains information such as whether it was a left or right click, which object was clicked on, and much more. IE Event objects are different than the objects created in modern browsers, so 


Internet Explorer 

```javascript
function handleClick(e) {
    if (!e) {
        e = window.event;
    }
    // do something here
}
```


---


One way of setting event listeners sometimes found in older IE code uses the following syntax

```html
<script for="myInput" event="onKeyDown">
    handleKeyEvent();
</script>
```

Of course the purpose of this code should be obvious. Here is the same code written in jQuery and standard Javascript

```javascript
//jQuery
$("#myInput").keydown(function(evt) {
    handleKeyEvent(evt);
});

//JavaScript
var temp = document.getElementById("myInput");
temp.addEventListener("keydown", function(evt) {
    handleKeyEvent(evt);
}, false);
```

---

Notice the name of the event in the IE code is "onload" and the jQuery method is "load". Most, but not all events have jQuery methods to match but without the "on" prefix. Here is a list of common jQuery methods to set event listeners:

* click
* keydown
* keyup
* mousedown
* mouseup
* hover
* load
* ready

---

## Custom Attributes

There are standard attributes that are allowed for HTML elements, such as `id="my-id"` or `class="example"`, but you may want to create your own custom attributes.

In HTML5 you can create custom attributes by prefixing the attribute name with "data-". In IE8 and older you could just make up an attribute and reference it the same way you would a normal attribute.

~~~html
<p id="test" class="hello" custom="world">
    lorem ipsum dolor sit amet...
</p>
<script type="text/javascript">
    var theClass = document.getElementById("test").class; // evaluates to "hello"
    var custom = document.getElementById("test").custom; // undefined
    var custom = document.getElementById("test").getAttribute("custom"); // evaluates to "world"
</script>
~~~









