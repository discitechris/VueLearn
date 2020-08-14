# Directives Introduction

{% hint style="info" %}
 Directives, are basically like HTML attributes which are added inside templates. They all start with ' **v- '**, to indicate that’s a Vue special attribute.
{% endhint %}

A directive is some special token in the markup that tells the library to do something to a DOM element.

 A Vue.js directive can only appear in the form of a prefixed HTML attribute that takes the following format:

```markup
<element prefix-directiveId="[argument:] expression"> </element>
<element prefix-directiveId="[argument:]"> </element>
```

A Simple Example

```markup
<div v-text="message"></div>
 <!-- Some directives can take an “argument”, denoted by a colon after the directive name. -->

```

This directive instructs Vue.js to update the div’s **`textContent`** whenever the **`message`** property on the Vue instance changes.

* Here the prefix is **`v`** which is the default. 
* The directive ID is **`text`**
* the expression is **`message`**

HTML:

```markup
<div id="app">
  {{ text }} Nice to meet Vue.
</div>
```

Vue:

```javascript
new Vue({
  el: "#app",
  data: {
    text: "Hello Tacoface!",
  },
});
```

```javascript
<!-- examples: -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]">
  <element prefix-directiveId="expression"> </element>
  <!-- examples: -->
  <div v-bind:id="'list-' + id"></div>
  <div v-for="(item, index) in items"></div>
  <div v-for="(val, key) in object"></div>
  <div v-for="(val, name, index) in object"></div>
  
<a v-bind:href="url"> ... </a>
<a v-on:click="doSomething"> ... </a>
 <!-- dynamic argument -->
a v-bind:[attributeName]="url"> ... </a>
<!-- attributeName will be evaluated as js expression -->
<!-- for ex: attributeName = "href" (string) or can use null to remove binding-->
<!--
This will be converted to v-bind:[someattr] in in-DOM templates.
Unless you have a "someattr" property in your instance, your code won't work.
-->
<a v-bind:[someAttr]="value"> ... </a>
```

