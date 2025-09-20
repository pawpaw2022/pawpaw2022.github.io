---
layout: about
title: about
permalink: /
subtitle: <a href='#'>Affiliations</a>. Address. Contacts. Motto. Etc.

profile:
  align: right
  image: prof_pic.jpg
  image_circular: false # crops the image to make it circular
  more_info: >
    <p>555 your office number</p>
    <p>123 your address street</p>
    <p>Your City, State 12345</p>

selected_papers: true # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page

announcements:
  enabled: true # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder

latest_posts:
  enabled: true
  scrollable: true # adds a vertical scroll bar if there are more than 3 new posts items
  limit: 3 # leave blank to include all the blog posts
---

<!-- About Section -->
<section id="about" class="section">
  <div class="container">
    <div class="row">
      <div class="col-lg-8">
        <h2 class="section-title">About</h2>
        <p>Write your biography here. Tell the world about yourself. Link to your favorite [subreddit](http://reddit.com). You can put a picture in, too. The code is already in, just name your picture `prof_pic.jpg` and put it in the `img/` folder.</p>
        
        <p>Put your address / P.O. box / other info right below your picture. You can also disable any of these elements by editing `profile` property of the YAML header of your `_pages/about.md`. Edit `_bibliography/papers.bib` and Jekyll will render your [publications page](/al-folio/publications/) automatically.</p>
        
        <p>Link to your social media connections, too. This theme is set up to use [Font Awesome icons](https://fontawesome.com/) and [Academicons](https://jpswalsh.github.io/academicons/), like the ones below. Add your Facebook, Twitter, LinkedIn, Google Scholar, or just disable all of them.</p>
      </div>
    </div>
  </div>
</section>

<!-- Research Section -->
<section id="research" class="section">
  <div class="container">
    <h2 class="section-title">Research</h2>
    <div class="publications">
      <!-- {% include bib_search.liquid %} -->
      {% bibliography %}
    </div>
  </div>
</section>

<!-- Projects Section -->
<section id="projects" class="section">
  <div class="container">
    <h2 class="section-title">Projects</h2>
    <div class="projects">
      {% if site.enable_project_categories %}
        <!-- Display categorized projects -->
        {% for category in site.display_categories %}
        <a id="{{ category }}" href=".#{{ category }}">
          <h3 class="category">{{ category }}</h3>
        </a>
        {% assign categorized_projects = site.projects | where: "category", category %}
        {% assign sorted_projects = categorized_projects | sort: "importance" %}
        <!-- Generate cards for each project -->
        <div class="row row-cols-1 row-cols-md-3">
          {% for project in sorted_projects %}
            {% include projects.liquid %}
          {% endfor %}
        </div>
        {% endfor %}
      {% else %}
        <!-- Display projects without categories -->
        {% assign sorted_projects = site.projects | sort: "importance" %}
        <div class="row row-cols-1 row-cols-md-3">
          {% for project in sorted_projects %}
            {% include projects.liquid %}
          {% endfor %}
        </div>
      {% endif %}
    </div>
  </div>
</section>

<!-- Blog Section -->
{% if site.sections.blog %}
<section id="blog" class="section">
  <div class="container">
    <h2 class="section-title">Blog</h2>
    <div class="post">
      {% assign blog_name_size = site.blog_name | size %}
      {% assign blog_description_size = site.blog_description | size %}

      {% if blog_name_size > 0 or blog_description_size > 0 %}
        <div class="header-bar">
          <h3>{{ site.blog_name }}</h3>
          <h4>{{ site.blog_description }}</h4>
        </div>
      {% endif %}

      {% if site.display_tags and site.display_tags.size > 0 or site.display_categories and site.display_categories.size > 0 %}
        <div class="tag-category-list">
          <ul class="p-0 m-0">
            {% for tag in site.display_tags %}
              <li>
                <i class="fa-solid fa-hashtag fa-sm"></i> <a href="{{ tag | slugify | prepend: '/blog/tag/' | relative_url }}">{{ tag }}</a>
              </li>
              {% unless forloop.last %}
                <p>&bull;</p>
              {% endunless %}
            {% endfor %}
            {% if site.display_categories.size > 0 and site.display_tags.size > 0 %}
              <p>&bull;</p>
            {% endif %}
            {% for category in site.display_categories %}
              <li>
                <i class="fa-solid fa-tag fa-sm"></i> <a href="{{ category | slugify | prepend: '/blog/category/' | relative_url }}">{{ category }}</a>
              </li>
              {% unless forloop.last %}
                <p>&bull;</p>
              {% endunless %}
            {% endfor %}
          </ul>
        </div>
      {% endif %}

      {% assign featured_posts = site.posts | where: "featured", "true" %}
      {% if featured_posts.size > 0 %}
        <br>
        <div class="container featured-posts">
          {% assign is_even = featured_posts.size | modulo: 2 %}
          <div class="row row-cols-{% if featured_posts.size <= 2 or is_even == 0 %}2{% else %}3{% endif %}">
            {% for post in featured_posts %}
              <div class="col mb-4">
                <a href="{{ post.url | relative_url }}">
                  <div class="card hoverable">
                    <div class="row g-0">
                      <div class="col-md-12">
                        <div class="card-body">
                          <div class="float-right">
                            <i class="fa-solid fa-thumbtack fa-xs"></i>
                          </div>
                          <h4 class="card-title text-lowercase">{{ post.title }}</h4>
                          <p class="card-text">{{ post.description }}</p>
                          {% if post.external_source == blank %}
                            {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
                          {% else %}
                            {% assign read_time = post.feed_content | strip_html | number_of_words | divided_by: 180 | plus: 1 %}
                          {% endif %}
                          {% assign year = post.date | date: "%Y" %}
                          <p class="post-meta">
                            {{ read_time }} min read &nbsp; &middot; &nbsp;
                            <a href="{{ year | prepend: '/blog/' | relative_url }}">
                              <i class="fa-solid fa-calendar fa-sm"></i> {{ year }} </a>
                          </p>
                        </div>
                      </div>
                    </div>
                  </div>
                </a>
              </div>
            {% endfor %}
          </div>
        </div>
        <hr>
      {% endif %}

      <ul class="post-list">
        {% assign postlist = site.posts | limit: 10 %}
        {% for post in postlist %}
          {% if post.external_source == blank %}
            {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
          {% else %}
            {% assign read_time = post.feed_content | strip_html | number_of_words | divided_by: 180 | plus: 1 %}
          {% endif %}
          {% assign year = post.date | date: "%Y" %}
          {% assign tags = post.tags | join: "" %}
          {% assign categories = post.categories | join: "" %}

          <li>
            {% if post.thumbnail %}
              <div class="row">
                <div class="col-sm-9">
            {% endif %}
              <h4>
                {% if post.redirect == blank %}
                  <a class="post-title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
                {% elsif post.redirect contains '://' %}
                  <a class="post-title" href="{{ post.redirect }}" target="_blank">{{ post.title }}</a>
                  <svg width="2rem" height="2rem" viewBox="0 0 40 40" xmlns="http://www.w3.org/2000/svg">
                    <path d="M17 13.5v6H5v-12h6m3-3h6v6m0-6-9 9" class="icon_svg-stroke" stroke="#999" stroke-width="1.5" fill="none" fill-rule="evenodd" stroke-linecap="round" stroke-linejoin="round"></path>
                  </svg>
                {% else %}
                  <a class="post-title" href="{{ post.redirect | relative_url }}">{{ post.title }}</a>
                {% endif %}
              </h4>
              <p>{{ post.description }}</p>
              <p class="post-meta">
                {{ read_time }} min read &nbsp; &middot; &nbsp;
                {{ post.date | date: '%B %d, %Y' }}
                {% if post.external_source %}
                  &nbsp; &middot; &nbsp; {{ post.external_source }}
                {% endif %}
              </p>
              <p class="post-tags">
                <a href="{{ year | prepend: '/blog/' | relative_url }}">
                  <i class="fa-solid fa-calendar fa-sm"></i> {{ year }} </a>
                {% if tags != "" %}
                  &nbsp; &middot; &nbsp;
                  {% for tag in post.tags %}
                    <a href="{{ tag | slugify | prepend: '/blog/tag/' | relative_url }}">
                      <i class="fa-solid fa-hashtag fa-sm"></i> {{ tag }}</a>
                    {% unless forloop.last %}
                      &nbsp;
                    {% endunless %}
                  {% endfor %}
                {% endif %}
                {% if categories != "" %}
                  &nbsp; &middot; &nbsp;
                  {% for category in post.categories %}
                    <a href="{{ category | slugify | prepend: '/blog/category/' | relative_url }}">
                      <i class="fa-solid fa-tag fa-sm"></i> {{ category }}</a>
                    {% unless forloop.last %}
                      &nbsp;
                    {% endunless %}
                  {% endfor %}
                {% endif %}
              </p>
            {% if post.thumbnail %}
              </div>
              <div class="col-sm-3">
                <img class="card-img" src="{{ post.thumbnail | relative_url }}" style="object-fit: cover; height: 90%" alt="image">
              </div>
            </div>
            {% endif %}
          </li>
        {% endfor %}
      </ul>
    </div>
  </div>
</section>
{% endif %}

<!-- CV Section -->
<section id="cv" class="section">
  <div class="container">
    <h2 class="section-title">CV</h2>
    <div class="cv-content">
      <p>Download my CV: <a href="{{ '/assets/pdf/example_pdf.pdf' | relative_url }}" target="_blank">PDF Version</a></p>
      <p>For a detailed CV, please download the PDF version above or visit the <a href="{{ '/cv/' | relative_url }}">CV page</a>.</p>
    </div>
  </div>
</section>
