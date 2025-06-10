---
layout: cv
title: CV
permalink: cv/
jsarr:
- js/scripts.js
---

<h1 id="cv-title"><a href="{{ site.url }}">Nicholas Papciak</a></h1>

<p id="cv-subtitle"><i>data engineer with a background in <span class="cv-vis">ml</span> + <span class="cv-ai">cs theory</span></i></p>


<div>
I build <b><span class="cv-vis">declarative data systems</span></b> using <b><span class="cv-ai">algorithmic foundations</span></b> to make pipelines more reliable and accessible.
</div>

<div class="cv-spacer"></div>


<div class="cv-spacer"></div>

<div class="cv-image-links-wrapper">
	<div class="cv-image-links">
		{% for link in site.data.social-links %}
			{% if link.cv-group == 1 %}
				{% include cv-social-link.html link=link %}
			{% endif %}
		{% endfor %}
	</div>
	<div class="cv-image-links">
		{% for link in site.data.social-links %}
			{% if link.cv-group == 2 %}
				{% include cv-social-link.html link=link %}
			{% endif %}
		{% endfor %}
	</div>
</div>

***

## Education

{::nomarkdown}
{% for degree in site.data.education %}
{% include cv/degree.html degree=degree %}
{% endfor %}
{:/}

## Experience

{% for experience in site.data.experiences %}
{% if experience.type == 'industry' %}
{% include cv/experience.html experience=experience %}
{% endif %}
{% endfor %}

## Leadership

{% for experience in site.data.experiences %}
{% if experience.type == 'leadership' %}
{% include cv/experience.html experience=experience %}
{% endif %}
{% endfor %}

## Honors and Awards

{% for award in site.data.awards %}
{% include cv/award.html award=award %}
{% endfor %}

<!-- ## Projects

{% assign featured_projects = site.projects | where: "featured", true %}
{% for project in featured_projects %}
  {% include cv/project.html project=project %}
{% endfor %} -->




## Tech

{% for skill in site.data.skills %}
{% include cv/skill.html skill=skill %}
{% endfor %}
