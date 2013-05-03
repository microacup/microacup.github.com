---
layout: index
title: 内涵不天黑！
tagline: 自由遥不可及
---
{% include JB/setup %}

######	01:22:45
<div class="panel index-fixed-info">
	<div class="alert alert-success span6 boxshadow">
		<p style="text-align:left">女神：你怎么还不睡？</p>
	</div>
	
	<div class="alert alert-info span6 boxshadow" style="margin-left: 50%">
		<p style="text-align:right">我：等我写完这段代码。</p>
	</div>
</div>

---

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