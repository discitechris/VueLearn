# v-for

Loops through a set of values

**Expects:** `Array` | `Object` | `number` | `string` | `Iterable` (since 2.6)

Render the element or template block multiple times based on the source data. The directive’s value must use the special syntax `alias in expression` to provide an alias for the current element being iterated on:

```html
<div v-for="item in items">
  {{ item.text }}
</div>
```
Alternatively, you can also specify an alias for the `index` (or the `key` if used on an `Object`):
```html
<div v-for="(item, index) in items"></div>
<div v-for="(val, key) in object"></div>
<div v-for="(val, name, index) in object"></div>
```

Inside `v-for` blocks we have full access to parent scope properties. `v-for` also supports an optional second argument for the `index` of the current `item`.

The default behavior of `v-for` will try to patch the elements in-place without moving them. To force it to reorder elements, you need to provide an ordering hint with the `key` special attribute:

```html
<div v-for="item in items" :key="item.id">
  {{ item.text }}
</div>
```

You can also use `of` as the delimiter instead of `in`, so that it is closer to JavaScript’s syntax for iterators


**Note:** When used together with `v-if`, `v-for` has a higher priority than `v-if`. 

## v-for with an Object

You can also use `v-for` to iterate through the properties of an object.

Html:
```html
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
<!-- You can also provide a second argument for the property’s name (a.k.a. key): -->
<div v-for="(value, name) in object">
  {{ name }}: {{ value }}
</div>
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```

Vue:
```js
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
})
```

Example:

Html:

```html
<div id="app">
  <ul>
    <li v-for="item in items">
      {{ item }}
    </li>
  </ul>
</div>
```

Vue:

```js
new Vue({
  el: "#app",
  data: {
    items: ["thingie", "another thingie", "lots of stuff", "yadda yadda"],
  },
});
```

Can also do a static number

HTML:

```html
<div id="app">
  <ul>
    <li v-for="num in 5" :key="num">
      {{ num }}
    </li>
  </ul>
</div>
```

Vue:

```js
Vue.config.devtools = true;

new Vue({
  el: "#app",
});
```

## `v-for` with a Range
`v-for` can also take an integer. In this case it will repeat the template that many times.
```html
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```
## `v-for` on a `<template>`

Similar to template `v-if`, you can also use a `<template>` tag with `v-for` to render a block of multiple elements.
```html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```

## Key attribute
It is recommended to provide a `key` attribute with `v-for` whenever possible, unless the iterated DOM content is simple, or you are intentionally relying on the default behavior for performance gains.

Since it’s a generic mechanism for Vue to identify nodes, the `key` also has other uses that are not specifically tied to `v-for`

**Expects:** `number` | `string` | `boolean` (since 2.4.2) | `symbol` (since 2.5.12)

The `key` special attribute is primarily used as a hint for Vue’s virtual DOM algorithm to identify VNodes when diffing the new list of nodes against the old list. Without keys, Vue uses an algorithm that minimizes element movement and tries to patch/reuse elements of the same type in-place as much as possible. With keys, it will reorder elements based on the order change of keys, and elements with keys that are no longer present will always be removed/destroyed.

Children of the same common parent must have **unique keys**. Duplicate keys will cause render errors.

The most common use case is combined with `v-for`

It can also be used to force replacement of an element/component instead of reusing it. This can be useful when you want to:

* Properly trigger lifecycle hooks of a component
* Trigger transitions

For example:

```html
<transition>
  <span :key="text">{{ text }}</span>
</transition>
```

When `text` changes, the `<span>` will always be replaced instead of patched, so a transition will be triggered.
