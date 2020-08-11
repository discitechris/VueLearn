# props

Type: `Array<string>` | `Object`

Details:

A list/hash of attributes that are exposed to accept data from the parent component. It has an Array-based simple syntax and an alternative Object-based syntax that allows advanced configurations such as type checking, custom validation and default values.

## Object-based Syntax

With Object-based syntax, you can use following options:

`type`: can be one of the following native constructors: `String`, `Number`, `Boolean`, `Array`, `Object`, `Date`, `Function`, `Symbol`, any custom `constructor function` or an `array` of those. Will check if a prop has a given type, and will throw a warning if it doesn’t.

`default`: any
Specifies a default value for the prop. If the prop is not passed, this value will be used instead. Object or array defaults must be returned from a factory function.

`required`: `Boolean`
Defines if the prop is required. In a non-production environment, a console warning will be thrown if this value is truthy and the prop is not passed.

`validator`: `Function`
Custom validator function that takes the prop value as the sole argument. In a non-production environment, a console warning will be thrown if this function returns a falsy value (i.e. the validation fails).

```js
// simple syntax
Vue.component("props-demo-simple", {
  props: ["size", "myMessage"],
});

// object syntax with validation
Vue.component("props-demo-advanced", {
  props: {
    // type check
    height: Number,
    // type check plus other validations
    age: {
      type: Number,
      default: 0,
      required: true,
      validator: function (value) {
        return value >= 0;
      },
    },
  },
});
```
