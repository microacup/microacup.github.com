---
layout: index
title: 内涵不天黑！
tagline: 自由遥不可及
---
{% include JB/setup %}

2013年4月27日1:22:45

	女神：你怎么还不睡？

	我：等我写完这段代码。

---
<div id="content">
    <div class="text-post posts">

	{% for post in site.posts limit:5 %}

		<h2><a class="post_title" href="{{ post.url }}">{{post.title}}</a></h2>

		<div class="caption rich-content">
			{{ post.content }}
		</div>

	{% endfor %}

    </div>
</div>