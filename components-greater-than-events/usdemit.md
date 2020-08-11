# $emit

```javascript
vm.$emit( eventName, […args] )
```

## Arguments:

* `{string} eventName`
* `[...args]`

Trigger an event on the current instance. Any additional arguments will be passed into the listener’s callback function.

## Examples:

### Using `$emit` with only an event name:

Html:

```markup
<div id="emit-example-simple">
  <welcome-button v-on:welcome="sayHi"></welcome-button>
</div>
```

Vue:

```javascript
new Vue({
  el: "#emit-example-simple",
  methods: {
    sayHi: function () {
      alert("Hi!");
    },
  },
});

Vue.component("welcome-button", {
  template: `
    <button v-on:click="$emit('welcome')">
      Click me to be welcomed
    </button>
  `,
});
```

### Using `$emit` with additional arguments:

Html:

```markup
<div id="emit-example-argument">
  <magic-eight-ball v-on:give-advice="showAdvice"></magic-eight-ball>
</div>
```

Vue:

```javascript
new Vue({
  el: "#emit-example-argument",
  methods: {
    showAdvice: function (advice) {
      alert(advice);
    },
  },
});

Vue.component("magic-eight-ball", {
  data: function () {
    return {
      possibleAdvice: ["Yes", "No", "Maybe"],
    };
  },
  methods: {
    giveAdvice: function () {
      var randomAdviceIndex = Math.floor(
        Math.random() * this.possibleAdvice.length
      );
      this.$emit("give-advice", this.possibleAdvice[randomAdviceIndex]);
    },
  },
  template: `
    <button v-on:click="giveAdvice">
      Click me for advice
    </button>
  `,
});
```

