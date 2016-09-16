---
layout: post
title:  "A Brief Introduction to Web Accessibility in Rails"
date:   2016-09-16 09:00:00
categories: code
---

What is Web Accessibility?
--------------------------
The Web Accessibility Initiative (WAI) provides the following definition:

>Web accessibility means that people with disabilities can use the Web. More specifically, Web accessibility means that people with disabilities can perceive, understand, navigate, and interact with the Web, and that they can contribute to the Web. Web accessibility also benefits others, including older people with changing abilities due to aging.

An large number of websites have significant barriers to access. These include (but are not limited to) unlabelled, or incorrectly labelled HTML tags

Why is Web Accessibility important?
--------------------------

The internet is increasingly important to many different aspects of our lives. It provides access to an incredible range of information and opportunities. Employment, education, recreation, communication -- all rely extensively on access to the internet.

Accessibility in Rails
--------------------------

Accessibility testing cannot be fully automated -- it requires evaluation and testing by an expert. However, there are some useful automated tools that will give you a basic idea of some of the issues with your website.

- [capybara-accessible](https://rubygems.org/gems/capybara-accessible/versions/0.2.1) is a gem used for automated accessibility testing. It runs an audit, using Capybara and Selenium, and running tests from the [Google Accessibility Developer Tools](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb?hl=en).

- [RAAKT](http://www.peterkrantz.com/raakt/wiki/) is another automated testing gem, that runs an audit to identify machine verifiable accessibility issues.

- [WAIAble]() is a gem that facilitates making accessible forms in Rails. For more information on accessible forms, see the [WCAG 2.0 guidelines]().

Simple Improvements
--------------------------

Here a couple of simple and easy examples of choices you can make to improve the accessibility of your app or website:

### Add alt-text to images

The alt attribute in an image tag should contain a concise description of the image. This attribute can be read by a screen reader, thereby conveying the meaning of an image.

If you're using raw HTML:


	<img src="flatiron-logo.png" alt="Flatiron School Logo"/>

If you're using the Rails <code>image_tag</code> helper:

	image_tag("flatiron-logo.png", alt: "Flatiron School Logo")

If you don't specify an alt attribute, the generated HTML from the image_tag with default to using the name of the image file, stripped and titleized.

### Don't use tables solely for layout purposes
From [Teach Access](https://teachaccess.github.io/tutorial/#/9):

>Tables help screen readers process information presented in a tabular format. When information is presented using table markup, screen reader users can read down columns and across rows, and even hear column and row headings as they do so.

If the information presented is intended to be read in a tabular manner -- use a table! If you're just using a grid for layout purposes, divs and CSS may be more appropriate.

Resources:
------
- [WCAG 2.0 Guidelines](https://www.w3.org/WAI/intro/wcag): Web Content Accessibility Guidelines are the technical standard for accessibility.
- [Teach Access](http://teachaccess.org/): Teach Access is an effort spearheaded by technology companies wishing to hire engineers, designers, and researchers who have been exposed to accessibility best practices
- [Teach Access Tutorial](https://teachaccess.github.io/): This tutorial from Teach Access provides an excellent introduction to basic accessibility issues for the developers and designers responsible for making mobile apps and websites accessible.


More to come!
------

This blog barely scratches the surface of the importance of web accessibility. I still have a lot to learn, and plan to write more in the future -- please let me know in the comments below if you have any advice, or any suggestions on what to cover next!
