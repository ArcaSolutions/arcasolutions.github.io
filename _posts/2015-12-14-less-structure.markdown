---
layout: post
title:  "Starting a Style Customization"
date:   2015-11-26 03:00:08
categories: custom
tags: [edirectory, custom-design]
---


---

## Introduction

eDirectory 11 is built with the Synfony structure to manage the files assets. In this case, [The Assetic library](http://symfony.com/doc/current/cookbook/assetic/index.html) is the responsible for creating the main style file, minimized to be included in the frontend pages. 

Therefore, the eDirectory 11 creates 3 style files for the project: 

1. **style.css** -> The result from assetic compilation of all .less files. 
2. **colorscheme.css** -> This file is used when the customer uses the feature *Color Options* on Site Manager. 
3. **csseditor.css** -> Available when the customer uses the feature *CSS Editor* on Site Manager.

---

## Working with Less 

You can use LESS to build the style for your custom theme. [The Assetic](http://symfony.com/doc/current/cookbook/assetic/index.html) is the one that handles with the LESS compilation in the Symfony structure. Inside the `base.html.twig` of your theme directory the stylesheets block will call all styles for your theme:

{% highlight php %}
{% raw %}
{% spaceless %}
   {% block stylesheets %}
       {% stylesheets
       filter='cssrewrite'
       output='assets/css/default/style.css'
       '%kernel.root_dir%/Resources/themes/default/assets/less/theme.less'
       'assets/js/lib/owl-carousel/*.css'
       '%kernel.root_dir%/Resources/themes/default/css/*.css' %}
       <link href="{{ asset_url }}" rel="stylesheet"/>
       {% endstylesheets %}
   {% endblock -%}
{% endspaceless -%}
{% endraw %}
{% endhighlight %}


Inside the folder of the project, run the command on terminal:

{% highlight bash %}
app/console assetic:watch 
{% endhighlight %}

This command will be waiting for any changes in the less files in the assets folder to recreate the compiled CSS files

Any css file inside the path `app/Resources/themes/default/css/` will be added to the minimized file `style.css` 

---

## Less Structure
The .less files are based on [Bootstrap Framework v3.3.4](https://github.com/twbs/bootstrap/releases/tag/v3.3.4) and only the main file is called by ASSETIC: `theme.less` (this file is the substitute for bootstrap.less) contains all @import including variables, mixins, plugins and resets. 

Start by setting the characteristics of the layout in `variables.less`. 

After importing the classic bootstrap files, we are including the eDirectory special styles:

Default eDirectory v11 Instalation:

{% highlight css %}
//eDirectory Components
@import "main-structure.less";
//The main elements are listed here. header, search, blocks and footer. 

@import "theme-box.less";
//The card structure to display eDirectory Items.

@import "reviews.less";
//Style for Reviews boxes and review rating

@import "social-links.less";
//Facebook buttons, social likes counter and any social interaction

@import "list-columns.less";
//Display list as columns with css3 rules

@import "calendar-events.less";
//Events calendar

@import "newsletter.less";
//Newsletter box style

@import "summary.less";
// All summary items layout

@import "detail.less";
// All detail pages layout

@import "special-pages.less";
// Faq and Sitemap

@import "login.less";
// Login Page style

@import "plans.less";
// Advertise Page Style for plans boxes

@import "members.less";
// Old structure for members layout

@import "scheme-members.less";
// Old structure for members layout colors

@import "order.less";
// Orders page

{% endhighlight %}

---

## Literature

[How to Use Assetic for Asset Management](http://symfony.com/doc/current/cookbook/assetic/asset_management.html)

[Assetic Guthub](https://github.com/kriswallsmith/assetic)

[Getting started with Less](http://lesscss.org/)
