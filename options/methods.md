# Methods

> **methods:** Methods of this vue instance

Type: `{ [key: string]: Function }`

Details:

Methods to be mixed into the Vue instance. You can access these methods directly on the VM instance, or use them in directive expressions. All methods will have their `this` context automatically bound to the Vue instance.

**Note** that you **should not use an arrow function** to define a method \(e.g. `plus: () => this.a++`\). The reason is arrow functions bind the parent context, so `this` will not be the Vue instance as you expect and `this.a` will be `undefined`.

```javascript
var vm = new Vue({
  data: { a: 1 },
  methods: {
    plus: function () {
      this.a++
    }
  }
})
vm.plus()
vm.a // 2
```

