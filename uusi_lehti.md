---
layout: default
permalink: /uusi_lehti/
---
<div class="page--magazine">
  {% include magazine.html %}
  <div class="article-list--magazine">
    {% for post in site.posts %}
      {% if post.magazine != site.latest_magazine %}
        {% continue %}
      {% endif %}
      <article class="article-list__teaser teaser">
        <div class="teaser__text">
          <h2>
            <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
          </h2>
          <span class="post-meta"><span class="post__category">{{ post.categories | join: ' ' | replace: '_', ' ' }}</span>{{ post.author }}</span>
        </div>
      </article>
    {% endfor %}
  </div>
</div>