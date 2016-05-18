---
layout: post
title:  "Changes thumbnails/logo size"
date:   2015-11-26 03:00:08
categories: custom
tags: [edirectory, custom-design]
---


---

## Thumbnails in LiipImagine

When you use the Assetic calling for images, you can also include a filter for this image. The [LiipImagine](https://github.com/liip/LiipImagineBundle) Bundle is responsible for manage the image filters and this bundle is set in the file `app/config/config.yml`. Keep in mind that all configuration inside the config.yml can be used in every domain. So, if you want to change the logo size directly in config.yml, be sure to create a new filter if you want to change only for one domain.

Let's see the logo example: 

{% highlight xml %}
{% raw %}
#Thumbnails Configuration
liip_imagine:    
    filter_sets:       
        logo:
            quality: 100
            filters:
                thumbnail:
                    size:
                        - 250
                        - 200
                    mode: inset
{% endraw %}
{% endhighlight %}