---
layout: home
title: Home
---




<div id="intro-wrapper" class="l-text">
	<div id="intro-title-wrapper">
		<div id="intro-image-wrapper">
			<!-- <img id="intro-image" src="/images/portrait.png"> -->
			{% include party-parrot.html %}
		</div>
		<div id="intro-title-text-wrapper">
			<h1 id="intro-title">Hi, I'm Nicholas</h1>
			<div id="intro-subtitle">engineer and cofounder at datafruit</div>
			<div id="intro-title-socials">
				{% for link in site.data.social-links %}
					{% if link.on-homepage == true %}
						{% include social-link.html link=link %}
					{% endif %}
				{% endfor %}
			</div>
		</div>
	</div>
	<div id="everything-else" class="l-middle">
		<a href="{{ site.url }}/cv"><div><i class="fa fa-portrait icon icon-right-space"></i>CV</div></a>
		<a href="{{ site.url }}/projects"><div><i class="fa fa-shapes icon icon-right-space"></i>Projects</div></a>
		<a href="{{ site.url }}/blog"><div><i class="fa fa-list-ul icon icon-right-space"></i>Blog</div></a>
	</div>
	<div>
	I am a 21 y/o aspiring computer scientist (<em>aspiring</em> because my career path has so far turned me into a software engineer).
	</div>
	<div style="height: 1rem"></div>
	<div>
	I did my undergrad at <img class="intro-logo" style="width: 7em; padding-left: 4px; padding-right: 5px;" src="/images/gt_logo.svg"> where I spent a lot of time taking niche cs theory classes and a little bit of machine learning. Based on my elite ball knowledge I was the head TA for algorithms for a few semesters. Meanwhile, I started grad school, but right after I finished my first semester I dropped out to work on a startup!
  </div>
	<div style="height: 1rem"></div>
	<div>
	That startup is <img class="intro-logo" style="width: 16px; padding-bottom: 5px; padding-left: 3px;" src="/images/datafruit_logo.svg"> <a href="https://datafruit.dev">datafruit</a>. We are backed by <span style="color: #ff6600; white-space: nowrap;">λf.(λx.f (x x)) (λx.f (x x))</span>
 and currently in the S25 batch.
</div>

<hr class="l-middle home-hr">

<h2 class="feature-title">Featured Blog Posts</h2>

<p class="feature-text">
    Some of my writings
</p>

<style>
.cover-image img {
  width: 100%;
  object-fit: cover;
  height: 180px;
  border-radius: 0.5rem;
}
.cover-wrapper-3-col {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 1.5rem;
}
.cover {
  background: #fff;
  border-radius: 0.75rem;
  box-shadow: 0 2px 6px rgba(0,0,0,0.1);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: box-shadow 0.2s ease, transform 0.2s ease;
}
.cover:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.1);
}
.cover-inner {
  padding: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  flex: 1;
}
.cover-title {
  font-weight: bold;
  font-size: 1rem;
  margin-bottom: 0.3rem;
}
.cover-subtitle {
  font-size: 0.875rem;
  color: #555;
  margin-bottom: 0.5rem;
}
.cover-venue {
  font-size: 0.75rem;
  color: #888;
}
.cover-links a {
  font-size: 0.75rem;
  text-decoration: none;
}
</style>

<div class="cover-wrapper cover-wrapper-3-col l-page">
  {% assign featured_posts = site.categories.blog | where: "featured", true %}
  {% for post in featured_posts %}
  <div class="cover">
    <div class="cover-image">
      <a href="{{ site.baseurl }}{{ post.url }}">
        {% if post.thumbnail %}
        <img src="{{ post.thumbnail }}">
        {% else %}
        <img src="/images/default.png">
        {% endif %}
      </a>
    </div>

    <div class="cover-inner">
      <div class="cover-top">
        <div class="cover-title">
          <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
        </div>

        {% if post.subtitle %}
        <div class="cover-subtitle">{{ post.subtitle }}</div>
        {% endif %}

        <div class="cover-venue">
          {{ post.date | date: "%B %Y" }}
        </div>
      </div>

      <div class="cover-footer-wrapper">
        <div class="cover-links">
          <span class="pub-misc">
            <a href="{{ site.baseurl }}{{ post.url }}">
              <i class="fas fa-book-open" aria-hidden="true"></i> Read More
            </a>
          </span>
        </div>
      </div>
    </div>
  </div>
  {% endfor %}
</div>
