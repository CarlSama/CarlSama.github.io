---
layout: page
title: Clint Wu 
description: "Clint Wu's Cottage"
---
<div class="row" id="home-page">
<article class="span8">
  {% for post in site.posts limit:5 %}
  <div class="row home-page-content">
    <aside class="span2">
	<div class="date">{{ post.date | date_to_string }}</div>
	<div class="tags">
	  <label>Tags:</label>{{ post.tags | array_to_sentence_string }}
	</div>
	<div class="category">
	  <label>Category:</label>{{ post.category }}
	</div>
    </aside>
    <article class="span6">
	<div class="title"><a href="{{ BASE_PATH  }}{{ post.url  }}">{{ post.title  }}</a></div>
	<div class="description">{{ post.description  }}</div>
	<div class="more"><a href="{{ BASE_PATH  }}{{ post.url  }} " class="btn">read more...</a></div>
    </article>
{%  if forloop.index != 5 %}
	<div class="post-footer">&nbsp;</div>
{% endif  %}
  </div>
  {% endfor  %}
</article>


<aside class="span4 ">
  <div class="side-bar">Category</div> 
  <div>
	<ul class="tag_box">
	{% assign categories_list = site.categories  %}
	{% include JB/categories_list   %}
	</ul>
  </div>
</aside>
</div>
<div class="home-page-footer"><a href="/archive.html">View all {{ site.posts.size  }} articles</a></div>
