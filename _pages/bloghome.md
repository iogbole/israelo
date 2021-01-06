---
layout: default
title: Blog Home
permalink: "/blog/index.html"

---

<div class="blog-grid-container">
  {% for post in site.posts %}
  {% include postbox.html %}
  {% endfor %}

</div>


<div class="bottompagination">
<span class="navigation" role="navigation">
    {% include pagination.html %}
</span>
</div>
