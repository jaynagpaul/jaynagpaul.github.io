---
layout: page
title: Archive
landing-title: 'Archive'
nav-menu: true
description: Collection of all thoughts and musings by Jay Nagpaul.
image: null
author: null
redirect_from: /tags/
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
				<p>{{post.description}}</p>
			</div>
		</a>
		{% endfor %}
		<div style="text-align: center; display:block; max-width:800px; margin:0 auto;">
		<h1>Tags</h1>
		{% capture temptags %}
  			{% for tag in site.tags %}
    			{{ tag[1].size | plus: 1000 }}#{{ tag[0] }}#{{ tag[1].size }}
  			{% endfor %}
		{% endcapture %}
		{% assign sortedtemptags = temptags | split:' ' | sort | reverse %}
		{% for temptag in sortedtemptags %}
  		{% assign tagitems = temptag | split: '#' %}
  		{% capture tagname %}{{ tagitems[1] }}{% endcapture %}
  		<a style="margin-left: 5px; margin-right: 5px;margin-bottom: 10px; width: 24%;" href="/tags/{{tagname}}" class="button special small">{{tagname}}</a>
		{% endfor %}
		</div>
	</div>
</section>

</div>
