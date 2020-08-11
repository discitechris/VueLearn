# v-text

Similar to using mustache templates

Updates the element’s `textContent`. If you need to update the part of `textContent`, you should use `{{ Mustache }}` interpolations.

**Expects:** `string`

**Warning:**

If you want to dynamically update, it's recommended to use mustache templates instead

HTML:

```html
<div id="app">
  <h3>What is your favorite kind of taco?</h3>
  <p v-text="tacos"></p>
  <p>{{ tacos }}</p>
  <!--mustache template -->
</div>
```

Vue:

```js
new Vue({
  el: "#app",
  data() {
    return {
      tacos: "I like Al Pastor tacos",
    };
  },
});
```
