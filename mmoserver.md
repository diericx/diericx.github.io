---
layout: page
title: MMO Server
permalink: /mmoserver/
project: mmoserver
platform: Go
summary: An MMO server I have been working on for a few years as a side project.
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
