---
title: Open Peer Power Frontend
description: Offers a frontend to Open Peer Power.
ha_category:
  - Other
ha_release: 0.7
ha_quality_scale: internal
ha_codeowners:
  - '@home-assistant/frontend'
ha_domain: frontend
excerpt: none
---

This offers the official frontend to control Open Peer Power. This integration is by default enabled, unless you've disabled or removed the [`default_config:`](https://www.openpeerpower.io/integrations/default_config/) line from your configuration. If that is the case, the following example shows you how to enable this integration manually:

{% highlight yaml %}
# Example configuration.yaml entry
frontend:
{% endhighlight %}

## Defining Themes

Starting with version 0.49 you can define themes:

{% highlight yaml %}
# Example configuration.yaml entry
frontend:
  themes:
    happy:
      primary-color: pink
    sad:
      primary-color: blue
{% endhighlight %}

The example above defined two themes named `happy` and `sad`. For each theme you can set values for CSS variables. For a partial list of variables used by the main frontend see [ha-style.ts](https://github.com/home-assistant/home-assistant-polymer/blob/master/src/resources/ha-style.ts).

Check our [community forums](https://community.openpeerpower.io/c/projects/themes) to find themes to use.

### Theme automation

There are 2 themes-related services:

 - `frontend.reload_themes`: reloads theme configuration from your `configuration.yaml` file.
 - `frontend.set_theme(name)`: sets backend-preferred theme name.

Example in automation:

Set a theme at the startup of Open Peer Power:

{% highlight yaml %}
automation:
  - alias: 'Set theme at startup'
    trigger:
     - platform: homeassistant
       event: start
    action:
      service: frontend.set_theme
      data:
        name: happy
{% endhighlight %}

To enable "night mode":

{% highlight yaml %}
automation:
  - alias: 'Set dark theme for the night'
    trigger:
      - platform: time
        at: '21:00:00'
    action:
      - service: frontend.set_theme
        data:
          name: darkred
{% endhighlight %}

### Manual Theme Selection

When themes are enabled in the `configuration.yaml` file, a new option will show up in the user profile page (accessed by clicking your user account initials at the bottom of the sidebar). You can then choose any installed theme from the dropdown list and it will be applied immediately.

<p class='img'>
  <img src='/images/frontend/user-theme.png' />
  Set a theme
</p>

## Loading extra HTML

Starting with version 0.53 you can specify extra HTML files to load, and starting with version 0.95 extra JS modules.

Example:

{% highlight yaml %}
# Example configuration.yaml entry
frontend:
  extra_html_url:
    - https://example.com/file1.html
    - /local/file2.html
  extra_module_url:
    - /local/my_module.js
{% endhighlight %}

HTML will be loaded via `<link rel='import' href='{{ extra_url }}' async>` on any page (states and panels), and modules via `<script type='module' scr='{{ extra_module }}'></script>`.

### Manual Language Selection

The browser language is automatically detected. To use a different language, go to the user profile page (accessed by clicking your user account initials at the bottom of the sidebar) and select one. It will be applied immediately.

<p class='img'>
  <img src='/images/frontend/user-language.png' />
  Choose a Language
</p>
