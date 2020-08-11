# Vue Instance

Every Vue application starts by creating a new Vue instance with the `Vue` function

When you create a Vue instance, you pass in an **options object**. 

For reference, you can also browse the full list of options in the [API reference](https://vuejs.org/v2/api/#Options-Data)

A Vue application consists of a root Vue instance created with new Vue, optionally organized into a tree of nested, reusable components. For example, a todo app’s component tree might look like this:

```html
Root Instance
└─ TodoList
   ├─ TodoItem
   │  ├─ DeleteTodoButton
   │  └─ EditTodoButton
   └─ TodoListFooter
      ├─ ClearTodosButton
      └─ TodoListStatistics
```

## Data and its Methods

When a Vue instance is created, it adds all the properties found in its `data` object to Vue’s **reactivity system**. When the values of those properties change, the view will “react”, updating to match the new values.

The only exception to this being the use of `Object.freeze()`, which prevents existing properties from being changed, which also means the reactivity system can’t track changes.

 If you know you’ll need a property later, but it starts out empty or non-existent, you’ll need to set some initial value.

```js
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})
```


# Interpolations

## Text

The most basic form of data binding is text interpolation using the **“Mustache”** syntax (double curly braces):

```js
<span>Message: {{ msg }}</span>
```
The mustache tag will be replaced with the value of the msg property on the corresponding data object. It will also be updated whenever the data object’s msg property changes.

The double mustaches interprets the data as plain text, not HTML.

Mustaches cannot be used inside HTML attributes.


## Using JavaScript Expressions

 Vue.js actually supports the full power of JavaScript expressions inside all data bindings:

 ```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
 ```

These expressions will be evaluated as JavaScript in the data scope of the owner Vue instance. One restriction is that each binding can only contain one single expression so the following will **NOT** work:
```html
<!-- this is a statement, not an expression: -->
{{ var a = 1 }}

<!-- flow control won't work either, use ternary expressions -->
{{ if (ok) { return message } }}
```