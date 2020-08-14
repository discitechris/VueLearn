# v-show

{% hint style="info" %}
**v-show**  is conditional directive triggers transitions when its condition changes.
{% endhint %}

**Expects:** `any`

* Toggles the element’s **`display:none`** CSS property based on the truthy-ness of the expression value.
* This can be anything- buttons, forms, divs, components.

{% hint style="warning" %}
**v-show** doesn’t support the `<template>` element, nor does it work with `v-else`.
{% endhint %}

HTML:

```markup
<div id="app">
  <h3>What is your favorite kind of taco?</h3>
  <textarea v-model="tacos"></textarea>

  <br />
  <button type="submit" v-show="tacos">Let us know!</button>
</div>
```

Vue:

```javascript
new Vue({
  el: "#app",
  data() {
    return {
      tacos: "",
    };
  },
});
```

