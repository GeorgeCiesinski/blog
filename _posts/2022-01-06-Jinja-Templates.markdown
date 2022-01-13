---
layout: single
title:  "Learning Jinja Templates"
date: 2022-01-06 23:00:00 -0500
categories: Bootstrap Flask Front-end Programming Text-Script
comments: true
typora-root-url: ..
---

Woah, I'm on a real roll! This marks the third day in a row I am blogging. Truth be told, I had a busy day and my brain feels like tenderized meat. Today, I worked on the CaterClock project I mentioned in a previous blog and was able to utilize Flask and Jinja to build a method called `show_cart()`. This method makes an API call to our API, gets the user object, extracts the orders, and populates the cart using Jinja.

# What is Jinja

[Jinja](https://jinja.palletsprojects.com/en/3.0.x/) is a templating engine that comes with Flask. It allows you to create `html` templates that are used to render web pages dynamically. These templates consist mainly of html code, but also contain python-like Jinja code, as well as variables which are provided by the python code which calls the Flask `templating.render_template()` function. In other words, it lets you use python code to generate a web page from an html template. Awesome!

# Jinja Template Syntax

A jinja template is stored in a file that can be called anything. For example, a template for the index page can be called `index.html` or even `index.jinja` as this is not a true html file, but a html  like template.

This template primarily uses the same syntax as an html file. In addition to html, you can also use jinja code. I will not go too in depth as the jinja documents do a far better job than I ever could, but you can get an idea of the basics below:

`{% command %}` - Curly brackets with % represent a jinja command. Some commands require a start and end block like this:

```
{% block block_name %}
  (do stuff)
{% block %}
```

`{{ input }}` - Double curly brackets represent an input. This executes the code inside the curly brackets and inputs the result into the html. It can be used to determine the url for example:

```html
<a href="{{ url_for('checkout.payment_form') }}"> Link </a>
```

`{# Comment #}` - Curly brackets with # represent a comment.

# Additional Jinja Commands

`{% include 'jinja_template.html' %}` - The include command can be used to inject another jinja template. In the below example, we are including the content in `head.html` within `index.html`:

```html
<!-- head.html -->
<title>Website Title</title>
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  {% include 'head.html' %}
</head>
```

`{% extends jinja_template %}` - Creates a child template of another template. This requires the block command.

`{% block block_name %}` - Creates a block with html/jinja code. Requires `{% end block %}` at the end.

This allows you to modify a parent template using content from a child template:

```html
<!-- Parent Template -->
<!doctype html>
<html>
  <head>
    <title>Website Title</title>
  </head>
  <body>
    {% block body %}
    {% end block %}
  </body>
</html>
```

```html
<!-- Child Template -->
{% extends 'parent_template.html' %}
{% block body %}
  <div class="container">Lorem ipsum. </div>
{% end block %}
```

`{% for item in items %}` - Creates a for loop and executes the following code. Requires `{% endfor %}`:

```
{% for order in orders %}
  (do stuff)
{% endfor %}
```

# Rendering Jinja Templates

Jinja templates can be rendered from a Flask method. For example, lets say you have a flask get method that is called when you try to access the website home page. You can actually render the template straight from the flask method using the `templating.render_template()` function. This function takes multiple parameters. The first is the path to the Jinja template you are rendering, and the remaining defines the variables to be used in the Jinja templates. After including it in the `render_template()` function, it does not need to be defined in the actual Jinja template.

For example, in the Flask GET method, you can render the jinja_template:

```python
return render_template(
  'templates/jinja_template.html',
  orders=orders,
)
```

In the jinja_template you are rendering, you can use the variable normally:


```
{% for order in orders %}
  (do stuff)
{% endfor %}
```

# Conclusion

Jinja templates are awesome because they let you use python like code along with variables to dynamically generate webpages.

If you read my previous blog about updating TextScript, this will be a vital component in that project which will allow me to render the text-block data from the API to the Bootstrap 5 elements in the web page.

Well, it is past midnight now, and tomorrow morning I have a meeting with the dev ops lead in my company. My manager was nice enough to set up an information exchange meeting with them so that I can share my development experience and get information about how the dev ops department in my company operates. With any luck, this might lead to an opportunity to dip my toes in a professional development environment, which could be valuable experience on future job applications.

So for now, I will say good night, and stay tuned for my next post!
