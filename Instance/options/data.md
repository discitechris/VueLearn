# data

> **data:** Store data to be used

Type: `Object` | `Function`

Restriction: Only accepts `Function` when used in a `component` definition.

Details:

The data object for the Vue instance. Vue will recursively convert its properties into **getter/setters** to make it **“reactive”**.

**The object must be plain** native objects such as browser API objects and prototype properties are ignored.

**Note**
A rule of thumb is that data should just be data - it is not recommended to observe objects with their own stateful behavior.

Once observed, you can no longer add reactive properties to the root data object. It is therefore recommended to declare all root-level reactive properties upfront, before creating the instance.

After the instance is created, the original data object can be accessed as `vm.$data`. The Vue instance also proxies all the properties found on the data object, so `vm.a` will be equivalent to `vm.$data.a`.

Properties that start with `_` or`$` will not be proxied on the Vue instance because they may conflict with Vue’s internal properties and API methods. You will have to access them as `vm.$data._`property.

**When defining a component, `data` must be declared as a function** that returns the initial `data` object, because there will be many instances created using the same definition. If we use a plain object for `data`, that same object will be **shared by reference** across all instances created! By providing a `data` function, every time a new instance is created we can call it to return a fresh copy of the initial data.

If required, a deep clone of the original object can be obtained by passing `vm.$data` through `JSON.parse(JSON.stringify(...))`.

Example:

```js
var data = { a: 1 };

// direct instance creation
var vm = new Vue({
  data: data,
});
vm.a; // => 1
vm.$data === data; // => true

// must use function when in Vue.extend()
var Component = Vue.extend({
  data: function () {
    return { a: 1 };
  },
});
```

Note that if you use an arrow function with the `data` property, `this` won’t be the component’s instance, but you can still access the instance as the function’s first argument:

```js
data: (vm) => ({ a: vm.myProp });
```
