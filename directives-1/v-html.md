# v-html

Great for strings that have html elements that need to be rendered!

**Expects:** `string`

**Details:**

Updates the element’s `innerHTML`. Note that the contents are inserted as plain HTML - they will not be compiled as Vue templates. If you find yourself trying to compose templates using `v-html`, try to rethink the solution by using components instead.

**Warnings:**

Dynamically rendering arbitrary HTML on your website can be very dangerous because it can easily lead to **XSS attacks**. Only use `v-html` on trusted content and **never use** on user-provided content.

**Warnings:**

In **single-file components**, `scoped` styles will not apply to content inside `v-html`, because that HTML is not processed by Vue’s template compiler. If you want to target `v-html` content with scoped CSS, you can instead use **CSS modules** or an additional, global `<style>` element with a manual scoping strategy such as **BEM**.

Don't use on user-rendered content, avoid XSS attacks HTML:

```markup
<div id="app">
  <h3>What is your favorite kind of taco?</h3>
  <p v-html="tacos"></p>
</div>
```

Vue:

```javascript
new Vue({
  el: "#app",
  data() {
    return {
      tacos: `I like <a href="http://www.epicurious.com/recipes/food/views/tacos-al-pastor-242132" target="_blank">Al Pastor</a> tacos`,
    };
  },
});
```

