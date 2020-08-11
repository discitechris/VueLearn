# v-if

## v-if

**Expects:** `any`

Is a conditional that will display information depending on meeting a requirement. This can be anything- buttons, forms, divs, components.

Conditionally render the element based on the truthy-ness of the expression value. The element and its contained directives / components are destroyed and re-constructed during toggles. If the element is a `<template>` element, its content will be extracted as the conditional block.

This directive triggers transitions when its condition changes.

**`v-if`** hides the entire html element by **replacing** it with a html comment tag

**Note:** When used together with `v-if`, `v-for` has a higher priority than `v-if`. Note that it’s **not recommended** to use `v-if` and `v-for` together.

HTML:

```markup
<div id="app">
  <h3>What is your favorite kind of taco?</h3>
  <textarea v-model="tacos"></textarea>

  <br />
  <button type="submit" v-if="tacos">Let us know!</button>
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

#### Conditional Groups with v-if on `<template>`

Because `v-if` is a directive, it has to be attached to a single element. But what if we want to toggle more than one element? In this case we can use `v-if` on a `<template>` element, which serves as an invisible wrapper. The final rendered result will not include the `<template>` element.

```markup
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

### v-if vs v-show

`v-if` is “real” conditional rendering because it ensures that event listeners and child components inside the conditional block are properly destroyed and re-created during toggles.

`v-if` is also **lazy**: if the condition is false on initial render, it will not do anything - the conditional block won’t be rendered until the condition becomes true for the first time.

In comparison, `v-show` is much simpler - the element is always rendered regardless of initial condition, with CSS-based toggling.

Generally speaking, `v-if` has higher toggle costs while `v-show` has higher initial render costs. So prefer `v-show` if you need to toggle something very often, and prefer `v-if` if the condition is unlikely to change at runtime.

## v-else & v-else-if

Pretty straightforward- you can conditionally render one thing or another.

A `v-else` element must immediately follow a `v-if` or a `v-else-if` element - otherwise it will not be recognized.

HTML:

```markup
<div id="app">
  <h3>Do you like tacos?</h3>
  <input type="radio" id="yes" value="yes" v-model="tacos" />
  <label for="no"> yes</label>
  <br />
  <input type="radio" id="no" value="no" v-model="tacos" />
  <label for="no"> no</label>

  <br />
  <div v-if="tacos">
    <p v-if="tacos === 'yes'" class="thumbs">👍</p>
    <p v-else>you're a monster</p>
  </div>
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

