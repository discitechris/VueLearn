# v-bind \(or\)  :

We can use it for so many things- class and style binding, creating dynamic props, etc

**Expects**: `any (with argument)` \| `Object (without argument)`

**Argument**: `attrOrProp` \(optional\)

**Usage:**

Dynamically bind one or more attributes, or a component prop to an expression.

When used to bind the `class` or `style` attribute, it supports additional value types such as Array or Objects.

When used for prop binding, the prop must be properly declared in the child component.

When used without an argument, can be used to bind an object containing attribute name-value pairs. **Note** in this mode `class` and `style` does not support Array or Objects

### Modifiers:

**`.prop`** - Bind as a DOM property instead of an attribute \(what’s the difference?\). If the tag is a component then `.prop` will set the property on the component’s `$el`.

**`.camel`** - \(2.1.0+\) transform the kebab-case attribute name into camelCase.

**`.sync`** - \(2.3.0+\) a syntax sugar that expands into a `v-on` handler for updating the bound value.

HTML:

```markup
<div id="app">
  <h3>What is your favorite kind of taco?</h3>
  <textarea v-model="tacos"></textarea>

  <br />
  <button :class="[tacos ? activeClass : '']">Let us know!</button>
</div>
```

Vue:

```javascript
new Vue({
  el: "#app",
  data() {
    return {
      tacos: "",
      activeClass: "active",
    };
  },
});
```

V-bind ShortHand:

```markup
<!-- full syntax -->
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a :[key]="url"> ... </a>
```

Examples:

```markup
<!-- bind an attribute -->
<img v-bind:src="imageSrc" />

<!-- dynamic attribute name (2.6.0+) -->
<button v-bind:[key]="value"></button>

<!-- shorthand -->
<img :src="imageSrc" />

<!-- shorthand dynamic attribute name (2.6.0+) -->
<button :[key]="value"></button>

<!-- with inline string concatenation -->
<img :src="'/path/to/images/' + fileName" />

<!-- class binding -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]">
  <!-- style binding -->
  <div :style="{ fontSize: size + 'px' }"></div>
    <!-- style binding with kebab casing if you prefer -->
   <div :style="{ 'font-size': size + 'px' }"></div>
   <!-- kebab-case can be converted to camelcase using latest modifier .camel -->
   <div :style.camel="{ 'font-size': size + 'px' }"></div>
  <div :style="[styleObjectA, styleObjectB]"></div>

  <!-- binding an object of attributes -->
  <div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

  <!-- DOM attribute binding with prop modifier -->
  <div v-bind:text-content.prop="text"></div>

  <!-- prop binding. "prop" must be declared in my-component. -->
  <my-component :prop="someThing"></my-component>

  <!-- pass down parent props in common with a child component -->
  <child-component v-bind="$props"></child-component>

  <!-- XLink -->
  <svg><a :xlink:special="foo"></a></svg>
</div>
```

## Class and Style Bindings

A common need for data binding is manipulating an element’s class list and its inline styles. Since they are both attributes, we can use v-bind to handle them: we only need to calculate a final string with our expressions. However, meddling with string concatenation is annoying and error-prone. For this reason, Vue provides special enhancements when v-bind is used with class and style. In addition to strings, the expressions can also evaluate to objects or arrays.

For more information [Click Here](https://vuejs.org/v2/guide/class-and-style.html)

