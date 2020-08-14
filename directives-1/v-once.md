# v-once

{% hint style="info" %}
Render the element and component **once** only.
{% endhint %}

 This can be used to optimize update performance.

* On subsequent re-renders, the element/component and all its children will be treated as static content and skipped. 
* Not quite as useful, v-once will not update once it's been rendered.

HTML:

```markup
<div id="app">
  <h3>What is your favorite kind of taco?</h3>
  <p><input v-model="tacos" /></p>
  <p v-once="tacos">{{ tacos }}</p>
  <pre>{{ $data }}</pre>
</div>
```

Vue:

```javascript
new Vue({
  el: "#app",
  data() {
    return {
      tacos: "I like Al Pastor tacos",
    };
  },
});
```

