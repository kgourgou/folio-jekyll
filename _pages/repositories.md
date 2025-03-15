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

{% if site.data.huggingface.huggingface_spaces or site.data.huggingface.huggingface_datasets or site.data.huggingface.huggingface_blog_posts %}
### Hugging Face Resources

<div class="row">
  {% if site.data.huggingface.huggingface_spaces %}
  <div class="col-md-4">
    <h4><i class="fa-solid fa-rocket"></i> Spaces</h4>
    <ul class="hf-list">
    {% for space in site.data.huggingface.huggingface_spaces %}
      {% assign space_name = space | split: "/" | last %}
      <li>
        <a href="https://huggingface.co/spaces/{{ space }}" target="_blank" rel="noopener">
          {{ space_name }}
        </a>
      </li>
    {% endfor %}
    </ul>
  </div>
  {% endif %}
  
  {% if site.data.huggingface.huggingface_datasets %}
  <div class="col-md-4">
    <h4><i class="fa-solid fa-database"></i> Datasets</h4>
    <ul class="hf-list">
    {% for dataset in site.data.huggingface.huggingface_datasets %}
      {% assign dataset_name = dataset | split: "/" | last %}
      <li>
        <a href="https://huggingface.co/datasets/{{ dataset }}" target="_blank" rel="noopener">
          {{ dataset_name }}
        </a>
      </li>
    {% endfor %}
    </ul>
  </div>
  {% endif %}
  
  {% if site.data.huggingface.huggingface_blog_posts %}
  <div class="col-md-4">
    <h4><i class="fa-solid fa-pen"></i> Blog Posts</h4>
    <ul class="hf-list">
    {% for post in site.data.huggingface.huggingface_blog_posts %}
      {% assign post_name = post | split: "/" | last | replace: "-", " " | capitalize %}
      <li>
        <a href="https://huggingface.co/blog/{{ post }}" target="_blank" rel="noopener">
          {{ post_name }}
        </a>
      </li>
    {% endfor %}
    </ul>
  </div>
  {% endif %}
</div>

<style>
  .hf-list {
    list-style-type: none;
    padding-left: 0;
  }
  .hf-list li {
    margin-bottom: 0.5rem;
  }
  .hf-list a {
    display: inline-block;
    padding: 0.25rem 0;
  }
</style>
{% endif %}
