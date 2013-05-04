---
layout: index
title: 内涵不天黑！
tagline: 自由遥不可及
---
{% include JB/setup %}

<div id="content">
    <div class="text-post posts">

	{% for post in site.posts limit:5 %}
	<div class="panel">
		<div class="header">
			<h4><a class="post_title" href="{{ post.url }}">{{post.title}}</a></h4>
		</div>
		<div class="caption inner">
			{{ post.content }}
		</div>
	</div>
	{% endfor %}

    </div>
</div>