---
title: "Glossary"
description: "Open Peer Power's Glossary."
---

{% assign entries = site.data.glossary | sort: 'topic'  %}

The glossary covers terms which are used around Open Peer Power.

<ul>
{% for entry in entries %}
  <li>
      <b>{{ entry.topic }}</b>: {{ entry.description | markdownify }}
  </li>
{% endfor %}
</ul>
