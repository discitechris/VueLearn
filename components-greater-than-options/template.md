# template

## template

**Type:** string

#### Details:

A string template to be used as the markup for the Vue instance. The template will **replace** the mounted element. Any existing markup inside the mounted element will be ignored, unless content distribution slots are present in the template.

If the string starts with `#` it will be used as a querySelector and use the selected element’s innerHTML as the template string. This allows the use of the common `<script type="x-template">` trick to include templates.

**Note:** From a security perspective, you should only use Vue templates that you can trust. Never use user-generated content as your template.

**Note:** If render function is present in the Vue option, the template will be ignored.

## 7 ways to Define a Component Template

There are plenty of choices when it comes to defining component templates in Vue. By my count, there are at least seven different ways!

### Plain strings

The quickest and easiest way to define a Vue component template is to add a `template` property to the component definition and assign a regular string containing your markup.

This method is really only used in code examples or quick prototypes, though, as it's very difficult to work with anything beyond the simplest template.

Vue:

```javascript
Vue.component("my-checkbox", {
  // use of strings in template
  template:
    '<div class="checkbox-wrapper" @click="check"><div :class="{ checkbox: true, checked: checked }"></div><div class="title">{{ title }}</div></div>',
  data() {
    return { checked: false, title: "Check me" };
  },
  methods: {
    check() {
      this.checked = !this.checked;
    },
  },
});
```

#### Details

* In HTML or JS? **JS**
* Needs polyfill or transpile? **No**
* Needs runtime template compiler? **Yes**

**Note:** **Runtime template compiler**: Vue comes with an internal module that is used to compile HTML templates to JavaScript at runtime. If you use a template option that does not use HTML markup at runtime you can use a special build of Vue.js that does not include this module \(and is, therefore, smaller and faster\).

### Template literals

As of ES2015, a special kind of string called a template literal can be declared using backticks. Unlike regular strings, these allow embedded expressions and can be multi-line.

The multi-line feature makes these much more useful for defining component templates compared to regular strings, as they make markup more readable.

Vue:

```javascript
Vue.component("my-checkbox", {
  template: `
    <div class="checkbox-wrapper" @click="check">
      <div :class="{ checkbox: true, checked: checked }"></div>
      <div class="title">{{ title }}</div>
    </div>
  `,
  data() {
    return { checked: false, title: "Check me" };
  },
  methods: {
    check() {
      this.checked = !this.checked;
    },
  },
});
```

#### Details

* In HTML or JS? **JS**
* Needs polyfill or transpile? **Yes**
* Needs runtime template compiler? **Yes**

**Note:** Older browsers may not support this ES2015 feature, so though you should probably transpile your code to be safe.

### X-templates

With this method, your template is defined inside a `<script>` tag in the _index.html_ file. The `<script>` tag is given the type **`type=text/x-template`** and referenced by **`id`** in your component definition.

On the plus side, this method allows you to write your template markup in an HTML file. The downside is that it separates the template from the rest of the component definition so it can be a little hard to reason about.

Vue:

```javascript
Vue.component("my-checkbox", {
  template: "#checkbox-template",
  data() {
    return { checked: false, title: "Check me" };
  },
  methods: {
    check() {
      this.checked = !this.checked;
    },
  },
});
```

Html:

```markup
<div id="app">...</div>
<script type="text/x-template" id="checkbox-template">
  <div class="checkbox-wrapper" @click="check">
    <div :class="{ checkbox: true, checked: checked }"></div>
    <div class="title">{{ title }}</div>
  </div>
</script>
```

#### Details

* In HTML or JS? **HTML**
* Needs polyfill or transpile? **No**
* Needs runtime template compiler? **Yes**

### Inline templates

With this method, you define the component's template within the parent template when it gets used. Just be sure to add the attribute **`inline-template`** so Vue knows where to find it.

The method has roughly the same upsides and downsides as x-templates. One interesting difference is that, since the template can be defined in the document body, the content could be crawled for SEO.

Vue:

```javascript
Vue.component("my-checkbox", {
  data() {
    return { checked: false, title: "Check me" };
  },
  methods: {
    check() {
      this.checked = !this.checked;
    },
  },
});
```

Html:

```markup
<div id="app">
  ...
  <my-checkbox inline-template>
    <div class="checkbox-wrapper" @click="check">
      <div :class="{ checkbox: true, checked: checked }"></div>
      <div class="title">{{ title }}</div>
    </div>
  </my-checkbox>
</div>
```

#### Details

* In HTML or JS? **HTML**
* Needs polyfill or transpile? **No**
* Needs runtime template compiler? **Yes**

### Render functions

Render functions require you to define your template using pure JavaScript. You'll need to read the Vue docs for the exact syntax, but the rough idea is that you define template nodes by calling **`createElement(tag, options, childElements)`**.

The advantage of doing this is that it requires no compilation of any sort, and gives you full access to JavaScript functionality rather than what's offered by directives. For example, to iterate within a markup template you can only use **`v-for`**, but in JavaScript, you can use any array method.

However, render functions are far more verbose and abstract than other template options and I don't expect many people would be comfortable writing a whole application like this.

Vue:

```javascript
Vue.component("my-checkbox", {
  data() {
    return { checked: false, title: "Check me" };
  },
  methods: {
    check() {
      this.checked = !this.checked;
    },
  },
  render(createElement) {
    return createElement(
      "div",
      {
        attrs: {
          class: "checkbox-wrapper",
        },
        on: {
          click: this.check,
        },
      },
      [
        createElement("div", {
          class: {
            checkbox: true,
            checked: this.checked,
          },
        }),
        createElement(
          "div",
          {
            attrs: {
              class: "title",
            },
          },
          [this.title]
        ),
      ]
    );
  },
});
```

#### Details

* In HTML or JS? **JS**
* Needs polyfill or transpile? **No**
* Needs runtime template compiler? **No**

### JSX

JSX is an extension of JavaScript that allows you to use a special template syntax in your JavaScript code.

Popularized by React, this is the most controversial template option in Vue, as some developers see it as ugly, unintuitive and as a betrayal to the Vue ethos.

However, JSX can be used to write a Vue render function in a far more readable and less abstract way. It does require you to transpile, though, as JSX is not readable by browsers.

Vue: **.jsx**

```jsx
Vue.component("my-checkbox", {
  data() {
    return { checked: false, title: "Check me" };
  },
  methods: {
    check() {
      this.checked = !this.checked;
    },
  },
  render() {
    return (
      <div class="checkbox-wrapper" onClick={this.check}>
        <div class={{ checkbox: true, checked: this.checked }}></div>
        <div class="title">{this.title}</div>
      </div>
    );
  },
});
```

#### Details

* In HTML or JS? **JS** \(**JSX**\)
* Needs polyfill or transpile? **Yes**
* Needs runtime template compiler? **No**

### Single-file components

One of the most popular features of Vue.js is the _**Single-File Component \(SFC\)**_. These allow you to write markup while keeping your component defintion in one file. They're compiled by vue-loader into render functions so you get the best runtime performance, too.

To create a SFC you first create a ._vue_ file, e.g. _Checkbox.vue_, then define your **`template`** in a **`<template>`** tag and define the **`component`** in a **`<script>`** tag. This file then gets imported into your main Vue app.

So long as you are comfortable using Vue CLI or setting up your own build tool in your project, SFCs are the way to go.

Vue: .**vue**

```javascript
<template>
  <div class="checkbox-wrapper" @click="check">
    <div :class="{ checkbox: true, checked: checked }"></div>
    <div class="title">{{ title }}</div>
  </div>
</template>
<script>
export default {
  data() {
    return { checked: false, title: 'Check me' }
  },
  methods: {
    check() { this.checked = !this.checked; }
  }
};
</script>
```

#### Details

* In HTML or JS? **HTML** \(**Vue**\)
* Needs polyfill or transpile? **Yes**
* Needs runtime template compiler? **No**

**TL;DR**

use **single-file components** as they're the most versatile and powerful option in almost every scenario.

