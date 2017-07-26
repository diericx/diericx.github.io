---
layout: page
title: Lettercraft
permalink: /lettercraft/
project: lettercraft
platform: Android
summary: Users have to form words from letters that fall from the top of the screen.
---

{{page.summary}}



<ul class="post-list">
    {% for post in site.posts %}
        {% if post.project == page.project %}
            <li>
                {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
                <span class="post-meta">{{ post.date | date: date_format }}</span>

                <h2>
                <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
                </h2>
            </li>
        {% endif %}
    {% endfor %}
</ul>
