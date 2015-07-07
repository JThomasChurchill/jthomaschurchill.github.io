---
layout: post
title:  "ADA Compliance"
date:   2015-04-09 10:20:18
categories: ADA
css_class: "tutorial"
---

<style>
  h3 {color: #4a549c;}
  img {box-shadow: 0px 0px 5px darkgray;}
  table {margin-bottom: 15px;}
  table caption {color: #58acfa; font-weight: bold;}
  th, td, tr {background-color: #fff; border: 1px solid #999; padding: 5px;}
</style>

---

## What is ADA?

The [Americans with Disabilities Act](http://en.wikipedia.org/wiki/Americans_with_Disabilities_Act_of_1990) (ADA) is a federal law that was passed to insure that, for example, public buildings are wheelchair accessible. Websites of course do not have wheelchair ramps, but there are other considerations.

Some visually impaired users have screen readers that will read out loud what is on the web page. These programs will not be able to make sense of the page if it is not written according to standards. Unfortunately many developers write a page and then load it in a browser to verify that it looks okay without ever actually looking at the resulting HTML source.

One good habit is to always run your work through a validator of some kind. For HTML there is the [Markup Validation Service](http://validator.w3.org/) made available by the World Wide Web Consortium (W3C). Some people prefer other validators but the important thing is to do more then simply make a visual check in your target browsers.

For ADA, HTML, and spelling validation I recommend [Total Validator](https://www.totalvalidator.com/). There is another tool called "Compliance Sheriff" (formerly "AccVerify"), but I have found it to be unstable and over-sensitive, giving many false positives even when it *was* working. 

One quick way to check for some basic compliance issues is to install the [Wave Toolbar](https://wave.webaim.org/toolbar/). It will catch the more common mistakes like images not having 'alt' text or labels not being properly associated with inputs.

---

## Tables

There are different kinds of tables in HTML, primarily “layout tables” and “data tables”. 

A layout table is typically invisible and is only used to structure a page, helping you to put things where you want them. This is a very old practice from the early days of the Internet before CSS and is very strongly discouraged. To be quite blunt about it pages using tables for layout purposes should be rewritten. Modern UI frameworks such as Bootstrap or Ionic offer grid layouts that are far more flexible than tables (I am literally begging you to read up on [responsive design](https://en.wikipedia.org/wiki/Responsive_web_design) if you are not already familiar with the term). A data table on the other hand is usually visible and used to display tabular data with rows and columns.

Data tables are ideal for displaying certain kinds of information in a neat and easily readable way, but they must be used correctly in order to be ADA compliant.

Consider the below example of a data table. The first row has three headers that describe their respective columns.

<table summary="First column shows name, second shows price, third shows if item is in stock">
  <caption>Produce Inventory</caption>
  <thead>
    <tr>
      <th scope="col">Item</th><th scope="col">Price</th><th scope="col">In Stock</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td> Apples </td><td> .50 each </td><td> True </td>
    </tr>
    <tr>
      <td> Oranges </td><td> .60 each </td><td> False </td>
    </tr>
    <tr>
      <td> Bananas </td><td> .20 each </td><td> True </td>
    </tr>
  </tbody>  
</table>


The above data table could be represented in HTML like so:

```html
<table summary="First column shows name, second shows price, third shows if item is in stock">
  <caption>Produce Inventory</caption>
  <tr>
    <th scope="col">Item</th><th scope="col">Price</th><th scope="col">In Stock</th>
  </tr>
  <tr>
    <td> Apples </td><td> .50 each </td><td> True </td>
  </tr>
  <tr>
    <td> Oranges </td><td> .60 each </td><td> False </td>
  </tr>
  <tr>
    <td> Bananas </td><td> .20 each </td><td> True </td>
  </tr>
</table>
```

There are three basic ingredients for an ADA compliant data table:

* Summary
* Caption
* Scope

A **summary** is a detailed description of the table, as you might describe it to someone who cannot see the screen. It is not displayed at all so it will have no visible effect. The summary attribute is not supported in HTML5, so it may be considered optional.

Although the **summary** element is now obsolete the concept of content that is only meant for screen readers is still important. The <code>sr-only</code> class in Bootstrap 3, for example, is for "screen readers only", and is useful if there are things you want to be available for accessibility reasons but without interfering with the visual layout of the page. Of course <code>display:none</code> might work too, but a class like <code>sr-only</code> is nice because it tells someone new to the project exactly why the element is being hidden.

A **caption** is the title of the table, and can be styled many different ways using CSS to meet your page design needs. You may want to set the caption to sr-only or display:none if you need to. This may help you keep the page ADA compliant while still conforming to your design specifications.

The **scope** indicates whether a header cell is describing a column or a row. The possible values for the scope attribute are col, row, colgroup, and rowgroup.

Below is an example of a table using <code>scope=”row”</code>.

<table>
  <caption>Info About Planets</caption>
  <tr>
    <th scope="row">Earth: </th>
    <td>blue in color</td>
    <td>3rd from the sun</td>
  </tr>
  <tr>
    <th scope="row">Mars: </th>
    <td>red in color</td>
    <td>4rth from the sun</td>
  </tr>
</table>

```html
<!-- obsolete 'summary' attribute omitted -->
<table>
  <caption>Info About Planets</caption>
  <tr>
    <th scope="row">Earth: </th><td>Blue in color</td><td>3rd from the sun</td>
  </tr>
  <tr>
    <th scope="row">Mars: </th><td>Red in color</td><td>4th from the sun</td>
  </tr>
</table>
```

---

## Labels

### Match the 'for' and 'id' attributes

Labels must only be used when they display text that describes an input element. A label is connected to an input element by making sure the label’s “for” attribute matches the input’s “id”. **Do not use a label unless it is linked to a form input**.

```html
<label for="first-name">First Name:</label>
<input id="first-name" type="text" name="firstName">
```

When this is done for radio or checkbox inputs, you can click on the text instead of directly on the button.

Only the button works if labels are broken.

![mouse clicking on circular radio button]({{ site.baseurl }}/images/clickOnRadioButtonInput.png)

Click on the text if labels are correct.

![mouse clicking on text label next to radio button]({{ site.baseurl }}/images/clickOnRadioButtonLabel.png)

### Using the Struts label tag

The Struts label tag can be problematic. If you use a Struts label you must set the text content with the key attribute instead of nesting the content inside an open and close tag like you would do with a standard HTML label.

**right:**

```html
<s:label for="first-name" key="your.key.firstname"/> <!-- correct use of key attribute -->
<input id="first-name" name="firstName"> <!-- 'id' and 'for' attribute values match -->
```

**wrong:**

```html
<s:label for="first-name"> <!-- 'for' attribute set correctly -->
  <s:text name="your.key.firstname" /> <!-- nested content will not render correctly -->
</s:label>
<input id="first-name" name="firstName"> <!-- 'id' and 'for' attribute values match -->
```

Depending on the Struts version or the template engine being used, the above example with a nested text tag inside a Struts label might render like this:

```html
<label for="first-name"></label>
First Name
<input id="first-name" name="firstName">
```

Notice that the label is empty and the text is directly underneath it! **Never just look at the page in your browser to see if it looks okay, verify the actual HTML source being rendered!**

### Use 'title' for groups of inputs

In some cases you may not be able to associate a label with text to every input element. In these cases you may use a <code>title</code> instead.

```html
<span>Phone Number:</span>
(<input name="phoneAreaCode" title="phone number area code">)
<input name="phoneFirstThree" title="first three digits of phone number">
- <input name="phoneLastFour" title="last four digits of phone number">
```

A <code>fieldset</code> can give an overall theme to a group of inputs using the <code>legend</code> element. If you do use a fieldset, you should only use them inside a form and always include a legend.

```html
<fieldset>
  <legend>Home Phone Number</legend>
  (<input name="areaCode" title="area code" maxlength="3">)
  <input name="firstThree" title="first three digits" maxlength="3">
  - <input name="lastFour" title="last four digits" maxlength="4">
</fieldset>
```

**Do not use labels to display text unless it is associated with an input element.**

---

## Images

All images should have alt text. If the image is a functional image the alternative text should describe the purpose of the image or be left empty depending on what will validate.

It is sometimes recommended to set the alt text to an empty string if the image is purely functional and not for display purposes. This may throw errors depending on your compliance validation tool: Compliance Sheriff will throw an error, but TotalValidator will not.

```html
<img src="/images/family.png" alt="Picture of a family">
<img src="/images/spacer.gif" alt="">
```

One way to include an image without a screen reader detecting it is to use a CSS background image. Since there is no way to associate alt text with a CSS background image, this method is not recommended if the image is important to a proper understanding of the page's contents.

```html
<div id="family-pic"></div>
```

```css
#family-pic {
  background-image: url('images/family.png');
  height: 100px;
  width: 200px;
}
```

---

## ID’s must be unique

Not just for ADA compliance, but to ensure JavaScript works properly, and because getting this wrong is in itself an HTML syntax error: **ALL ID’S MUST BE UNIQUE**.

This shouldn’t be difficult to manage, but one complication that may cause confusion is the fact that custom tags (Struts is a big offender!) that generate HTML elements might automatically generate an id for you. Struts tags will create ID’s based on the form id or form name if no form id is specified. So for example…

This...

```html
<form id="form1">
  <s:a href="/faq.html">Click here for the FAQ</s:a>
  <s:a href="/policy.html">Click here for policy info</s:a>
  <s:a id="help-link" href="/help.html">Click here for help</s:a>
</form>
```

...will show up something like this

```html
<form id="form1">
  <a id="form1_" href="/faq.html">Click here for the FAQ</a>
  <a id="form1_" href="/policy.html">Click here for policy info</a>
  <a id="help-link" href="/help.html">Click here for help</a>
</form>
```

**Be sure to specify an ID for all Struts tags that generate HTML elements to avoid duplicate ID’s.**

Alternatively, if you can just use regular HTML instead of a Struts tag, you probably should.

---

## Forms

All forms must have at least one submit button. Multiple forms on a page can cause errors, especially with older browsers. It is a good idea to give each form a unique id, but the name attribute is obsolete and should be omitted. There are three valid types of submit button:

* input type="submit"
* input type="image"
* button type="submit"

```html
<input type="image" value="Submit" src="images/icon.jpg" alt="Submit">
<input type="submit" value="Submit">
<button type="submit">Submit Button</button>
```

Note that the Struts submit tag &lt;s:submit /&gt; will generate &lt;input type="submit"&gt;.

---

## Summary: Do's and Don'ts

---

### Tables

**Do**

* use the scope attribute for &lt;th&gt; elements
* use a caption
* use tables for displaying data in a list or grid

**Do Not**

* use tables for layout purposes
* nest a table inside another table
* insert ANYTHING inside of a table unless it is properly inside a &lt;td&gt; or &lt;th&gt; element

---

### Forms

**Do**

* Provide a unique id
* Provide at least one submit button
* Make sure any fieldset is used inside a form

**Do Not**

* Use the obsolete name attribute
* Use a fieldset outside of a form

---

### Labels

**Do**

* Provide a for attribute with a value that matches the id of the associated input
* Use the key attribute for Struts label tags

**Do Not**

* Use a label without an associated input
* Use name attributes for labels
* Allow an empty label to appear on the page

---

### Inputs

**Do**

* provide a unique id
* use a title attribute if a label cannot be associated with the input
* it is usually good practice to make the name and id of an input the same

**Do Not**

* allow an input to appear on a page without either a title or an associated label

---

### Fieldsets

**Do**

* use fieldsets to provide a context for a group of related inputs
* always include a legend when using a fieldset

**Do Not**

* use a fieldset outside of a form

---

### Images

**Do**

* use the alt attribute on all images, even if you leave it empty

**Do Not**

* use a CSS background image if including alt text would be more appropriate
