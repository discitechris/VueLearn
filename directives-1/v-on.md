# v-on \(or\) @

{% hint style="info" %}
 **`v-on`** allows you to listen to DOM events, and trigger a method when the event happens.
{% endhint %}

**Expects:** `Function` \| `Inline Statement` \| `Object`

**Argument:** `event`

```markup
<!-- method/function handler -->
<button v-on:click="doThis"></button>

<!-- inline statement with argument-->
<button v-on:click="doThat('hello', $event)"></button>

<!-- object syntax (2.4.0+) -->
<button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
```

* When used on a **normal element**, it listens to **native DOM events** only. 
* When used on a **custom element component**, it listens to **custom events** emitted on that child component.

When listening to native DOM events, the method receives the native event as the only argument. If using inline statement, the statement has access to the special `$event` property: `v-on:click="handle('ok', $event)"`.

Starting in 2.4.0+, `v-on` also supports binding to an object of event/listener pairs without an argument. Note when using the object syntax, it does not support any modifiers.

HTML:

```markup
<div id="app">
  <div class="item">
    <img
      src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/28963/backpack.jpg"
      width="235"
      height="300"
    />
    <div class="quantity">
      <button class="inc" @click="counter > 0 ? counter -= 1 : 0">-</button>
      <span class="quant-text">Quantity: {{ counter }}</span>
      <button class="inc" @click="counter += 1">+</button>
    </div>
    <button class="submit" @click="">Submit</button>
  </div>
  <!--item-->
</div>
```

Vue:

```javascript
new Vue({
  el: "#app",
  data() {
    return {
      counter: 0,
    };
  },
});
```

## Multiple Bindings

Can use multiple bindings on single attribute id

```markup
<div
  v-on="
 click   : onClick,
 keyup   : onKeyup,
 keydown : onKeydown
"
></div>
<div
  @="
 click   : onClick,
 keyup   : onKeyup,
 keydown : onKeydown
"
></div>
```

{% hint style="info" %}
> #### We can also use ternaries directly
>
> **`<a @click="[isActive ? doSomething: somethingElse ]"> ... </a>`**
{% endhint %}

## Custom Element Components

Listening to custom events on a child component \(the handler is called when “my-event” is emitted on the child\):

```markup
<my-component @my-event="handleThis"></my-component>

<!-- inline statement -->
<my-component @my-event="handleThis(123, $event)"></my-component>

<!-- native event on component -->
<my-component @click.native="onClick"></my-component>
```

## 'V-on Modifiers

**`.once`** not to be confused with v-once, this click event will be triggered once.

**`.stop`** - call `event.stopPropagation()`.

**`.prevent`** - call `event.preventDefault()`.

**`.capture`** - add event listener in capture mode.

**`.self`** - only trigger handler if event was dispatched from this element.

**`.{keyCode | keyAlias}`** - only trigger handler on certain keys.

**`.native`** - listen for a native event on the root element of component.

**`.left`** - \(2.2.0+\) only trigger handler for left button mouse events.

**`.right`** - \(2.2.0+\) only trigger handler for right button mouse events.

**`.middle`** - \(2.2.0+\) only trigger handler for middle button mouse events.

**`.passive`** - \(2.3.0+\) attaches a DOM event with { `passive: true` }.

v-on Shorthand

```markup
<!-- full syntax -->
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a @[event]="doSomething"> ... </a>


```

Example:

```markup
<!-- dynamic event (2.6.0+) -->
<button v-on:[event]="doThis"></button>

<!-- shorthand -->
<button @click="doThis"></button>

<!-- shorthand dynamic event (2.6.0+) -->
<button @[event]="doThis"></button>

<!-- stop propagation -->
<button @click.stop="doThis"></button>

<!-- prevent default -->
<button @click.prevent="doThis"></button>

<!-- prevent default without expression -->
<form @submit.prevent></form>

<!-- chain modifiers -->
<button @click.stop.prevent="doThis"></button>

<!-- key modifier using keyAlias -->
<input @keyup.enter="onEnter" />

<!-- key modifier using keyCode -->
<input @keyup.13="onEnter" />

<!-- the click event will be triggered at most once -->
<button v-on:click.once="doThis"></button>
```

