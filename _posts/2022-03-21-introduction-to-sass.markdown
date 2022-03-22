---
layout: single
title: "Introduction to SASS"
date: 2022-03-21 23:00:00 -0500
categories: Programming Bootstrap Front-end
comments: true
typora-root-url: ..
---

# Life update

Hello world. It has been a few months since my last blog post, and a lot of changes have happened. First of all, my new room mates and I were able to find a new place to live. We were hoping for a house, but home owners typically reject applicants who are not families. This search was a bit frustrating because despite being four working professionals with four separate incomes, we got rejected from many beautiful homes. But in the end, we ended up finding a condo that I am really happy with, so maybe it worked out! We cut it right down to the wire.

The move was pretty challenging on the other hand. I had a bad idea and thought it would be smart to move all the small things a few days early, then rent a small cargo van to move the big stuff. On the day I was moving the big stuff, my entire body was in pain and I barely finished on time. To make matters worse one of our neighbours stole my cat tree! After this, I spent an entire month moving small stuff that wasn't moved before. Either way, long story short, I decided to hire movers in the future.

Anyways, onto my long overdue blog post!

# What is CSS

CSS stands for "Cascading Style Sheets". While HTML is used to create the content of web pages, CSS is used to define the colors, layouts and styles of the various elements of the page.

# What is SASS

SASS stands for "Syntactically Awesome Style Sheets", and it is a CSS extension language. SASS contains more advanced features and is the most common way to modify the design of Bootstrap web pages.

# Bootstrap SASS

As per the Bootstrap 5 [docs](https://getbootstrap.com/docs/5.0/customize/sass/), it is not recommended to mess with the Bootstrap core files. From personal experience, it is also a bad idea to modify the CSS files directly as this can become difficult to maintain. I have personal experience with this mistake, which caused very strange behaviour on a mobile site I worked and my css was being ignored by some models of iPhone. I was unable to resolve the issue on Stack Overflow because the most common answer was people asking me why I'm not using SASS.

# Writing SASS

A SASS file is a file that ends in the extension `.scss`. In the SASS file, you can actually include normal CSS, as well as SASS specific statements. These include top level statments, Universal statements, CSS statements, and other kinds of statements.

In the Bootstrap 5 [docs](https://getbootstrap.com/docs/5.0/customize/sass/), every single page of the docs has a SASS section at the bottom which shows the various variables you can change. It also shows mixins and examples of how to actually use it. With these commands it is possible to alter nearly every part of a Bootstrap 5 web page.

It is important to note that the Bootstrap 5 docs warn against modifying core Bootstrap sass files. Instead, you should write all of your custom SASS in a new `.scss` file.

# Compiling SASS

Websites cannot read SASS so once you have included all of your customization you need to actually compile the `.scss` file into usable CSS. You can do this using an app like Koala. To use the app successfully, you need to open up your SASS directory, and specify an output path - this is usually the CSS file you linked in your HTML.

# Conclusion

This was a short blog as I am getting back into the groove. This is a very basic SASS explanation and I have left out literally 99% of information about it. This is by design as I am still learning many of the details and putting it to use. What I can say for certain is that I am in love with SASS and find it 100x easier than writing CSS manually. I will be releasing blog posts more regularly now that I am coding more frequently, so I will see you in my next post!
