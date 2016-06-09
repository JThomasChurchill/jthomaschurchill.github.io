---
layout: post
title:  "Handlebars Cheatsheet"
date:   2016-06-04 10:00:00
categories: JavaScript
css_class: "tutorial"
---

## Handlebars

This cheat sheet is not just about "Handlebars" in general, but specifically about the flavor of Handlebars you are working with if you are using the Node.js [express-generator](http://expressjs.com/en/starter/generator.html) module with the <code>--hbs</code> option. The github page for the Handlebars implementation used on this page is [here](https://github.com/donpark/hbs).

## Configuration

Basic configurations should be made in the <code>app.js</code> file. To do this a reference to the Handlebars implementation called "hbs" must first be made:

~~~javascript
var exphbs = require('hbs');
~~~

Once this reference has been established, partials and helpers can be registered. The declaraton <code>app.set('view engine', 'hbs');</code> is generated for you by the **express-generator** scaffolding engine, and that's okay: you don't have to interfere with that generated code to do what you want to do.

---

## Partials

A partial is like a mini-template that can be used inside of a helper. Helpers are where all the logic is, so partials are only dumb templates that take variables, but don't have any iteration or conditional capabilities.

Partials can be declared most easily by specifying which directory they are all located in. A "views" directory is created for you by express-generator. I recommend you create a "partials" directory inside of it and then register whatever partials you create in that directory. You should use either the <code>.hbs</code> or <code>.html</code> file extensions for your partials.

Spaces and dashed in Partial names will be converted to underscores:

file name | partial
--------- | ----------
login view.hbs | \{\{\> login_view\}\}
template-file.html | \{\{\> template_file\}\}

~~~javascript
exphbs.registerPartials(__dirname + '/views/partials');
~~~

As for the files you generate in the partials directory, I have used the file extensions ".html" and ".hbs". I don't know or care if any other exstensions work.

---

## Helpers

### Registering Helpers In app.js

~~~javascript
exphbs.registerHelper('caps', function(text) { ... });
exphbs.registerHelper('list', function(array, options) { ... });
~~~

### Modularize Your Helpers

Don't be **that guy**, don't lump everything into one file. You really should put all your helper functions in a module and import them into app.js.

#### /custom_modules/helpers/helpers.js
~~~javascript
var Handlebars = require('hbs');

var caps = function(text) {
	return (text)? text.toUpperCase() : "";
};

var list = function(array, options) {
	var listElement = "ul";
	if (options.hash.ordered && options.hash.ordered == true) {
		listElement = "ol";
	}
	var output = "options.hash = "+ Object.keys(options.hash) +"<br>";
	output += "options.data = "+ Object.keys(options.data) +"<br>";
	output += "<"+ listElement +">";
	for (var i in array) {
		output += "<li>"+ array[i] +"</li>";
	}
	output += "<li>"+ options.data.foo +"</li>";
	output += "</"+ listElement +">";
	return new Handlebars.SafeString(output);
};

module.exports.list = list;
module.exports.caps = caps;
~~~

#### app.js
~~~javascript
var hbs_helpers = require('./custom_modules/helpers');
exphbs.registerHelper('caps', hbs_helpers.caps);
exphbs.registerHelper('list', hbs_helpers.list);
~~~

---

## Application Data

You can use app.locals properties as Template data. What are app.locals properties? Well, fuck you.

#### app.js
~~~javascript
var app = express();

/* set variables to be accessed in templates */
app.locals.foo = "bar";
app.locals.language = "en";

// tell Handlebars to use application data
exphbs.localsAsTemplateData(app);
~~~

In templates you can access these application data using \{\{@foo\}\}. So for example \<html lang="\{\{@language\}\}" \>

