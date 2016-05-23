---
layout: post
title:  "Starting a Style Customization"
date:   2015-11-26 03:00:08
categories: custom
tags: [edirectory, custom-design]
---


---

## Introduction

eDirectory 11 is built with the PHP framework **Symfony**, which has its own way no manage assets files. The library [Assetic](http://symfony.com/doc/current/cookbook/assetic/index.html) is responsible for creating the main stylesheet file to be included in all frontend pages. It's important to mention here that this file is already minified and should not be edited while customizing a theme.

eDirectory 11 works with 3 main stylesheet files:

1. **style.css** -> The result of the compilation of all .less files by Assetic.
2. **colorscheme.css** -> This file is used when the customer uses the feature *Color Options* on Site Manager. 
3. **csseditor.css** -> Available when the customer uses the feature *CSS Editor* on Site Manager.

---

## Working with Less 

You can use LESS to customize your theme. On the file `base.html.twig`, the block stylesheets will call all files for your theme:

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

<link href="{{ asset(getDomainParameter('path') ~ 'theme/customtheme/colorscheme.css') }}" rel="stylesheet"/>

{% if file_exists(getDomainParameter('path') ~ 'theme/customtheme/csseditor.css') -%}
   <link href="{{ asset(getDomainParameter('path') ~ 'theme/customtheme/csseditor.css') }}" rel="stylesheet"/>
{% endif -%}
{% endraw %}
{% endhighlight %}

Now, open your terminal, go to the root folder of your project and run the following command:

{% highlight bash %}
app/console assetic:watch 
{% endhighlight %}

This command will be watching the assets folder and will recompile the main stysheet file everytime any changes are done in one of the LESS files.

Any css file on the folder `app/Resources/themes/default/css/` will be automatically added to the minimized file `style.css`.

---

## Less Structure
eDirectory .less files are based on the frontend framework [Bootstrap v3.3.4](https://github.com/twbs/bootstrap/releases/tag/v3.3.4) and only the main file `theme.less` is called by ASSETIC. This file replaces bootstrap.less and contains all @import including variables, mixins, plugins and resets.

Start by setting the main characteristics of the layout on the file `variables.less`. This file determines the colors and sizes used on mixins and functions. Read the file carefully before editing any variable and don’t forget to update the default colors in the colorscheme file: `default_themecolors.inc.php`

After importing the main bootstrap files, eDirectory special styles are included:

Default eDirectory v11 Instalation:

{% highlight css %}
//eDirectory Components
@import "main-structure.less";
//The main elements are listed here. header, search, blocks and footer. 

@import "theme-box.less";
//The card structure to display eDirectory Items.

@import "reviews.less";
//Style for Reviews boxes and reviews rating

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

## HTML Structure

To customize an eDirectory theme, you'll be working on the following folders:

1) `/edirectory/app/Resources/themes/default`: The main structure of the theme on Symfony's structure, using [Twig](http://twig.sensiolabs.org/), a template engine for PHP. Below are listed the main files that you should be familiar with:

{% highlight php %}

base.html.twig: base html structure, including the main tags html, head and body.

index.html.twig: structure for the home page

maintenance.html.twig: maintenance page

results.html.twig: results page

/assets/less: all .less files are here

/blocks: the pages are structured in smaller blocks that can be found on this folder

/modules: Twigs for each module, split mainly on index, results and detail

/pages: secondary pages like contact us, faq, site map, etc

{% endhighlight %}

2) `/edirectory/web/theme/default`: eDirectory still has some legacy code that was not migrated to Symfony yet. The profile and members sections are one of those code. For the profile section, it has the same header and footer as the frontend structure, and the HTML is duplicated on this folder.

------

## Literature

[How to Use Assetic for Asset Management](http://symfony.com/doc/current/cookbook/assetic/asset_management.html)

[Assetic Guthub](https://github.com/kriswallsmith/assetic)

[Getting started with Less](http://lesscss.org/)
