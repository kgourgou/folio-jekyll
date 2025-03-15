---
layout: page
permalink: /repositories/
title: repositories
description: My projects and contributions on GitHub and Hugging Face
nav: true
nav_order: 4
---

## GitHub

{% if site.data.repositories.github_users %}

<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for user in site.data.repositories.github_users %}
    {% include repository/repo_user.liquid username=user %}
  {% endfor %}
</div>

---

{% if site.repo_trophies.enabled %}
{% for user in site.data.repositories.github_users %}
{% if site.data.repositories.github_users.size > 1 %}

  <h4>{{ user }}</h4>
  {% endif %}
  <div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% include repository/repo_trophies.liquid username=user %}
  </div>

---

{% endfor %}
{% endif %}
{% endif %}

{% if site.data.repositories.github_repos %}
### Repositories

<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for repo in site.data.repositories.github_repos %}
    {% include repository/repo.liquid repository=repo %}
  {% endfor %}
</div>
{% endif %}

## Hugging Face

{% if site.data.huggingface.huggingface_users %}
<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for user in site.data.huggingface.huggingface_users %}
    {% include huggingface/user.liquid username=user %}
  {% endfor %}
</div>

---
{% endif %}

{% if site.data.huggingface.huggingface_spaces %}
### Spaces

<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for space in site.data.huggingface.huggingface_spaces %}
    {% include huggingface/space.liquid space=space %}
  {% endfor %}
</div>
{% endif %}

{% if site.data.huggingface.huggingface_datasets %}
### Datasets

<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for dataset in site.data.huggingface.huggingface_datasets %}
    {% include huggingface/dataset.liquid dataset=dataset %}
  {% endfor %}
</div>
{% endif %}

{% if site.data.huggingface.huggingface_blog_posts %}
### Blog Posts

<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for post in site.data.huggingface.huggingface_blog_posts %}
    {% include huggingface/blog_post.liquid post=post %}
  {% endfor %}
</div>
{% endif %}
