---
layout: default
permalink: /uusi_lehti/
---
{% for post in site.posts %}
  {% if post.magazine != site.latest_magazine %}
    {% continue %}
  {% endif %}
  <article class="article-list__teaser teaser">
    {% if post.image %}
    <a href="{{ post.url | prepend: site.baseurl }}">
      <img class="teaser__image" src="{{ post.image | prepend: site.baseurl }}">
    </a>
    {% endif %}
    <div class="teaser__text">
      <span class="post-meta"><span class="post__category">{{ post.categories | join: ' ' | replace: '_', ' ' }}</span> {{ post.date | date: "%d.%m.%Y" }}</span>
      <h2>
        <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
      </h2>
    </div>
  </article>
{% endfor %}