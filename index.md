---
layout: page
title: Index
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
		    	<span class="badge">
		    		<time>{{ post.date | date: "%-d %B %Y" }}</time>
		    	</span>
	    	</span>
	    </li>
	{% endfor %}
</ul>

