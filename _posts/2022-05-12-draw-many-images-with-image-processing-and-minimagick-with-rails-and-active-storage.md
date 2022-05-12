---
title: Draw many images with image_processing and minimagick with rails and active
  storage
layout: post
categories:
- dev
tags:
- ruby
- rails
- image_processing
- minimagick
date: '2022-05-12 09:50:54'
---

Adding only one filigrane is pretty simple with image processing, you can do it like that:

{% highlight ruby %}
watermark_image_path = Rails.root.join(
  "app", "assets", "images", "watermark.png"
)
self.image.variant(
  resize_to_fill: [500, 500], 
  gravity: "center",
  draw: "image Over 0,0 0,0 \"#{watermark_image_path}\""
)
{% endhighlight %}

But if you want to draw more than one filigrane, image processing doesn't handle it by its own, you have to use the `append` method which let you put as many draws as you want in the convert command:

{% highlight ruby %}
watermark_image_path = Rails.root.join(
  "app", "assets", "images", "watermark.png"
)
self.image.variant(
  resize_to_fill: [500, 500], 
  gravity: "center",
  append: [
    "-draw", "image Over 0,0 0,0 \"#{watermark_image_path}\"",
    "-draw", "image Over 30,0 0,0 \"#{watermark_image_path}\"",
    "-draw", "roundrectangle 16, 5, 304, 85 20,40"
  ]
)
{% endhighlight %}
