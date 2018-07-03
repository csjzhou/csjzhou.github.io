---
layout: page
title: Research
show-avatar: true
published: true
---


----
## Research Interests
- Explainable AI
- Human and Machine Learning
- Human Factors (Cognitive Load, Trust, Confidence)
- Machine Learning
- Visual Analytics
- Augmented Reality


----
<!---
<div style="width:600px; height:400px; overflow:scroll; overflow-x: hidden; overflow-y: hidden;">
      <a href="http://ifip-tc13.org/awards/">
        <img style=" float:center; display:inline"  src="/img/INTERACT2017_Award_The Brian Shackel Award.jpg" 
             title="The Brian Shackel Award" 
             width="250" height="200" alt="best paper award" hspace="20" /></a>
      <a href="/img/INTERACT2017_Award_small.jpg">
        <img style=" float:center; display:inline"  src="/img/INTERACT2017_Award_small.jpg" 
             title="Reviewers' Choice Award"
             width="200" height="300" alt="Reviews' Choice Award" hspace="20" /></a>
 </div>
--->
---------- 
[![BSA](/img/INTERACT2017_Award_Brian_Shackel.jpg){:height="200px" width="250px"}](http://ifip-tc13.org/awards/)
----------
----------
[![RCA](/img/INTERACT2017_Award_small.jpg){:height="200px" width="250px"}](/img/INTERACT2017_Award_small.jpg)
----------

----




<div class="posts-list">
  {% for post in site.tags.research-project %}
  <article class="post-preview">
    <a href="{{ post.url | prepend: site.baseurl }}">
      <h3 class="post-title">{{ post.title }}</h3>
      {% if post.subtitle %}
      <h3 class="post-subtitle">
        {{ post.subtitle }}
      </h3>
      {% endif %}
    </a>
    <p class="post-meta">
      Posted on {{ post.date | date: "%B %-d, %Y" }}
    </p>
    <div class="post-entry-container">
      {% if post.image %}
      <div class="post-image">
        <a href="{{ post.url | prepend: site.baseurl }}">
          <img src="{{ post.image }}">
        </a>
      </div>
      {% endif %}
      <div class="post-entry">
        {{ post.excerpt | strip_html | xml_escape | truncatewords: site.excerpt_length }}
        {% assign excerpt_word_count = post.excerpt | number_of_words %}
        {% if post.content != post.excerpt or excerpt_word_count > site.excerpt_length %}
          <a href="{{ post.url | prepend: site.baseurl }}" class="post-read-more">[Read&nbsp;More]</a>
        {% endif %}
      </div>
    </div>
    {% if post.tags.size > 0 %}
    <div class="blog-tags">
      Tags:
      {% if site.link-tags %}
      {% for tag in post.tags %}
      <a href="{{ site.baseurl }}/tags#{{ tag }}">{{ tag }}</a>
      {% endfor %}
      {% else %}
        {{ post.tags | join: ", " }}
      {% endif %}
    </div>
    {% endif %}

   </article>
  {% endfor %}
</div>


