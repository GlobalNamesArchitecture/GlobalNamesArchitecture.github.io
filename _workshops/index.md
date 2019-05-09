---
layout: default
title: Workshops
date: 2019-05-9 18:39:28
type: workshop
permalink: /workshops/
---
<section class="news">
  <div class="grid">
  <div class="unit four-fifths">
    {% include docs_contents_mobile.html %}
    {% for section in site.data.workshops %}
      {% for item in section.docs %}
        {% assign item_url = item |prepend:"/workshops/" |append:"/" %}
        {% for page in site.workshops %}
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
            {% include workshops_content.html %}
              </article>
          {% endif %}
        {% endfor %}
      {% endfor %}
    {% endfor %}

  </div>

    {% include workshops_content.html %}

    <div class="clear"></div>

  </div>
</section>
