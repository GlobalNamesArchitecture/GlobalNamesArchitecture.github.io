---
layout: default
title: Applications and libraries
date: 2015-08-28 18:39:28
type: apps
permalink: /apps/
---
<section class="news">
  <div class="grid">
  <div class="unit four-fifths">
    {% include docs_contents_mobile.html %}
    {% for section in site.data.apps %}
      {% for item in section.docs %}
        {% assign item_url = item |prepend:"/apps/" |append:"/" %}
        {% for page in site.apps %}
          {% if page.url == item_url %}
   <article>
   {% if page.date %}
   <div class="app-date">Edited on: {{ page.date | date_to_string }}</div>
   {% endif %}
   <h2>
    {{ page.title }}
    <a href="{{ page.url }}" class="permalink" title="Permalink">∞</a>
   </h2>
            {{ page.content }}
            {% include apps_content.html %}
              </article>
          {% endif %}
        {% endfor %}
      {% endfor %}
    {% endfor %}

  </div>

    {% include docs_contents.html %}

    <div class="clear"></div>

  </div>
</section>
