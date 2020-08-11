# v-pre

v-pre will print out the inner text exactly how it is, including code \(good for documentation\)

Skip compilation for this element and all its children. Skipping large numbers of nodes with no directives on them can speed up compilation.

HTML:

```markup
<div id="app">
  <h3>What is your favorite kind of taco?</h3>
  <p><input v-model="tacos" /></p>
  <span v-pre
    >This is good if I need to show the mustache view of {{ tacos }}</span
  >
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

