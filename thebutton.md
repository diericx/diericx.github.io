---
layout: page
title: Emoji Dash
permalink: /thebutton/
project: thebutton
summary: A simple button gambling app. Users try to match emojis and the first one to match all 4 wins. The winner gets to send all the players a picture.
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