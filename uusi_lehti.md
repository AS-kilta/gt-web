---
layout: default
permalink: /uusi_lehti/
---
<div class="page--magazine">
  <div class="sidebar__new-magazine teaser teaser--magazine">
    {% include magazine.html %}
    <div>
      <br>
      <p>{{ site.latest_magazine_description }}</p>
    </div>
  </div>
  <div class="article-list--magazine">
    {% assign posts = (site.posts | where: 'magazine', site.latest_magazine | sort: 'print_order', 'last') %}
    {% for post in posts %}
      <article class="article-list__teaser teaser">
        <div class="teaser__text">
          <h2>
            <a class="post-link" href="{{ post.url | prepend: site.baseurl  | replace:'%C3%A4','ä' }}">{{ post.title }}</a>
          </h2>
          <span class="post-meta"><span class="post__category">{{ post.categories | join: ' ' | replace: '_', ' ' }}</span>{{ post.author }}</span>
        </div>
      </article>
    {% endfor %}
  </div>
</div>