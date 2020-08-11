# Introduction

Vue is a progressive JavaScript framework that focuses on building user interfaces. As it only works in the “view layer” it makes no assumption of middleware and backend and therefore can be integrated easily into other projects and libraries. Vue.js offers a lot of functionality for the view layer and can be used for building powerful single-page webapps. In the following you can find a list of features:

* Reactive Interfaces
* Declarative Rendering
* Data Binding
* Directives
* Template Logic
* Components
* Event Handling
* Computed Properties
* CSS Transitions and Animations
* Filters

The Vue.js 2 core library is very small in size \(only 17 kB\). This ensures that the overhead which is added to your project by using Vue.js is minimal and your website is loading fast.



```markup
<div id="app">
{{ text }} Nice to meet Vue.
</div>
```

```javascript
new Vue({
  el: '#app',
  data: {
    text: 'Hello World!'
  }
});
```

```text
Output: 
Hello World! Nice to meet Vue.
```

* **new Vue\(\)** creates a vue instance
* **el** object - element tag targets \#ids, .class, tagnames any css selectors.
* **text** object - establishes a relationship with the html using `{{}}` mustache tags

