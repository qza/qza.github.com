---
layout: page
title: index
tagline: Welcome note
---
{% include JB/setup %}

<ul class="blank-list">
	{% for post in site.posts %}
		<li>
			<span>
		   		<a href="{{ post.url }}">
		   			{{ post.title }}
		    	</a>
	    	</span>
	    </li>
	{% endfor %}
</ul>

