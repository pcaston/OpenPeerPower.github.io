---
title: "Shopping List Card"
sidebar_label: Shopping List
description: "The Shopping List card allows you to add, edit, check-off, and clear items from your shopping list."
excerpt: none
---

The Shopping List card allows you to add, edit, check-off, and clear items from your shopping list.

Setup of the [Shopping List Intent](/integrations/shopping_list/) is required

<p class='img'>
<img src='/images/lovelace/lovelace_shopping_list_card.gif' alt='Screenshot of the shopping list card'>
Screenshot of the Shopping List card.
</p>

{% highlight yaml %}
type: shopping-list
{% endhighlight %}

{% configuration %}
type:
  required: true
  description: shopping-list
  type: string
title:
  required: false
  description: Title of Shopping List
  type: string
theme:
  required: false
  description: "Set to any theme within `themes.yaml`"
  type: string
{% endconfiguration %}

## Examples

Title Example:

{% highlight yaml %}
type: shopping-list
title: Shopping List
{% endhighlight %}
