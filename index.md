---
layout: index
title: 内涵不天黑！
tagline: 自由遥不可及
---
{% include JB/setup %}

######	01:22:45

<div class="alert alert-success span6">
	<p style="text-align:left">女神：你怎么还不睡？</p>
</div>

<div class="alert alert-info span6 offset6">
	<p style="text-align:right">我：等我写完这段代码。</p>
</div>

<br />
<br />
<br />
<br />
<br />
<br />
<br />

---

<div id="content">
    <div class="text-post posts">

	{% for post in site.posts limit:5 %}
	<div class="panel" style="background-color: #fff">
		<h2><a class="post_title" href="{{ post.url }}">{{post.title}}</a></h2>

		<div class="caption rich-content">
			{{ post.content }}
		</div>
	</div>
	{% endfor %}

    </div>
</div>