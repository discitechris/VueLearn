# Computed

> **computed:** Dependent properties

Type: `{ [key: string]: Function | { get: Function, set: Function } }`

Details:

Computed properties to be mixed into the Vue instance. All getters and setters have their `this` context automatically bound to the Vue instance.

Note that if you use an arrow function with a computed property, `this` won’t be the component’s instance, but you can still access the instance as the function’s first argument:

```js
computed: {
  aDouble: (vm) => vm.a * 2;
}
```

**Computed properties are cached, and only re-computed on reactive dependency changes**.

**Note** that if a certain dependency is out of the instance’s scope (i.e. not reactive), the computed property **will not be** updated.

```js
var vm = new Vue({
  data: { a: 1 },
  computed: {
    // get only
    aDouble: function () {
      return this.a * 2;
    },
    // both get and set
    aPlus: {
      get: function () {
        return this.a + 1;
      },
      set: function (v) {
        this.a = v - 1;
      },
    },
  },
});
vm.aPlus; // => 2
vm.aPlus = 3;
vm.a; // => 2
vm.aDouble; // => 4
```
