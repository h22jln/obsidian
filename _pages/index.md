---
layout: page
title: Home
id: home
permalink: /
---

# 안녕하세요 🍟

<p style="padding: 3em 1em; background: #f5f7ff; border-radius: 4px;">
  신희진의 옵시디언 메모장입니다
</p>

<strong>Recently updated notes</strong>

<ul>
  {% assign recent_notes = site.notes | sort: "last_modified_at_timestamp" | reverse %}
  {% for note in recent_notes limit: 5 %}
    <li>
      {{ note.last_modified_at | date: "%Y-%m-%d" }} — <a class="internal-link" href="{{ site.baseurl }}{{ note.url }}">{{ note.title }}</a>
    </li>
  {% endfor %}
</ul>

***
## Git
[[Git 시작하기]]
