---
title: Custom Panel
description: Instructions on how to add customized panels to the frontend of Open Peer Power.
ha_category:
  - Front End
ha_release: 0.26
ha_quality_scale: internal
ha_codeowners:
  - '@home-assistant/frontend'
ha_domain: panel_custom
excerpt: none
---

The `panel_custom` support allows you to add additional panels to your Open Peer Power frontend. The panels are listed in the sidebar if wished and can be highly customized. See the developer documentation on [instructions how to build your own panels](https://developers.home-assistant.io/docs/en/frontend_creating_custom_panels.html).

To enable customized panels in your installation, add the following to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
panel_custom:
  - name: my-panel
    sidebar_title: TodoMVC
    sidebar_icon: mdi:work
    url_path: my-todomvc
    js_url: /local/my-panel.js
    config:
      who: world
{% endhighlight %}

<div class='note'>

Store your custom panels in `<config>/www` to make them available in the frontend at the path `/local`.

</div>
