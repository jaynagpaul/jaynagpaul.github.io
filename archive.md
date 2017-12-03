---
layout: page
title: Archive
landing-title: 'Archive'
nav-menu: true
description: Collection of all posts written.
image: null
author: null
---
<!-- Main -->
<div id="main" class="alt">
<!-- One -->
<section id="one">
	<div class="inner">
		{% for post in site.posts %}
		<a href = "{{ post.url  | relative_url }}">
			<div class="box">
				<header class="major">
					<h1>{{ post.title }}</h1>
				</header>
				{% if post.date %}<p>{{ post.date | date_to_string}}</p>{% endif %}
				<p>{% if post.description %}{{post.description}}{% else %}{{ post.content | strip_html | truncatewords: 100 }}{% endif %}</p>
			</div>
		</a>
		{% endfor %}
	</div>
</section>

</div>
