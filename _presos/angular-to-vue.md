---
title: "Angular to Vue"
date: 2018-08-16
excerpt: >-
  Vue JS represents a spiritual successor to Angular JS: it shares much of the
  same template syntax and basic paradigm and is familar and accessible to devs
  with Angular JS experience. But how does a team, practicaly, migrate from one
  core framework to another without rewriting _everything_ in the process?
---
<script type="text/template">

# Angular JS to Vue JS

## Without a stop-the-presses rewrite...

----

{% include profile.slide.html %}

----

# This is an _interactive_ presentation.

### Please laugh. Please ask questions.

----

# What is Angular JS?

### A modular, client-side JavaScript framework for developing dynamic, reactive web applications.

----

## What is Angular JS?

#### A modular, client-side JavaScript framework for developing dynamic, reactive web applications.

- __modular__ -- comprised of many _encapsulated modules_ of functionality; encourages the same
- __client-side__ -- residing and executing on the _client_ machine, i.e. in the browser not on the server
- __JavaScript framework__ -- not a full application; pieces to _make one_ with JavaScript
- __dynamic__ -- data-driven, interactive user interfaces (UIs) like dashboards and data tables
- __reactive__ -- not requiring a full-page refresh for form submissions or view changes: a Single Page App (SPA)
- __web application__ -- not a web _site_ or _page_ that is typically textual content with some static assets (see _dynamic_)

----

### What is Angular JS?

{% include aside.img.html
  src="angularjs-1.7-LTS-announced.png"
%}

<p> ![RIP Angular JS: 2009 - 2018](https://i.imgflip.com/2fzmlo.jpg) <br> **Effectively Dead** </p>
<!-- .element: class="fragment" -->


----

## &#x2728; What is Vue JS? &#x2728;

- Really just the _rendering engine_ of Angular JS... With some inspiration from React, Riot, etc
- The _**View**_ in **Model-View-Controller**; the _**View + ViewModel**_ in **Model-View-ViewModel**
- Basic "reactivity"... and not much more...

### Does _NOT_ provide:

- NO Dependency Injection (DI) system: no `$injector` or `service` constructors
- NO Model implementation (beyond in-memory): no `$http` or `$resource`
- NO sanitization or contextual-escaping mechanism: no `$sce` or `$sanitize`

----

# So let's put 'em together!

![You got peanut butter in my chocolate...](https://78.media.tumblr.com/e31237ed9e524ff3980537487dd7cba3/tumblr_o2aeoqDdRX1tooympo1_400.gif)
![nuclear detonation](https://media.giphy.com/media/X92pmIty2ZJp6/giphy.gif)
<!-- .element class="fragment" -->

### What could go wrong?

----

### What could go wrong?

- Name collisions between Angular components and Vue components: WHO WILL WIN?
- Vue JS uses the same template delimiters as Angular JS
- How do we write Models? How do we reuse our _existing_ Models?
- So what if we use `$scope` / `$rootScope` a lot? Is that a problem?

----

# DEMO

----

### Vue JS + Angular JS

<aside class="pull-left">
{% capture list-left %}
#### Pros &#x1f601; <!-- .element: style="font-weight:normal" -->

- BIG performance gains over Angular JS!
- Team velocity gains!
- Incremental upgrades!
- Copy-paste-tweak!
- Webpack HMR + Angular HARD. Vue JS: _&#x2714;_
{% endcapture %}{{list-left | markdownify }}
</aside>

<aside class="pull-right">
{% capture list-left %}
#### Cons &#x1f613; <!-- .element: style="font-weight:normal" -->

- Patchwork context switching...
- Multiple Vue instances...
- Final state is hard to visualize...
- Remember to `$digest`...
- Bridging `$scope` / `vuex` / `redux`
- VERY LARGE GRAPH BAD
{% endcapture %}{{list-left | markdownify }}
</aside>

----

{% include profile.slide.html %}

</script>
