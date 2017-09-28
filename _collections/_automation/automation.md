---
layout: default
title: Automation
---

{% for automation in site.automation %}


<a href="{{ automation.url | prepend: site.baseurl }}">
        <h2>{{ automation.title }}</h2>
</a>

{% endfor %}      
