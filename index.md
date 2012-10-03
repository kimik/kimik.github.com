---
layout: page
title: Hello World!
tagline: Supporting tagline
---
{% include JB/setup %}


## Update Author Attributes

    title : Devik's Blog =)
    
    author :
      name : Harry Kim
      email : x_terc@nate.com
      github : kimik
      twitter : @x_terc

The theme should reference these variables whenever needed.

Here's a sample "posts list".

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



## To-Do

This theme is still unfinished. If you'd like to be added as a contributor, [please fork](http://github.com/plusjade/jekyll-bootstrap)!
We need to clean up the themes, make theme usage guides with theme-specific markup examples.


Read [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

Complete usage and documentation available at: [Jekyll Bootstrap](http://jekyllbootstrap.com)


