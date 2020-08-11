# v-model

## v-model

Creates a relationship between the data in the instance/component and a form input, so you can dynamically update values

**Expects:** varies based on value of form inputs element or output of components

**Limited to:**

* `<input>`
* `<select>`
* `<textarea>`
* components

**v-model** internally uses different properties and emits different events for different input elements:

* **text** and **textarea** elements use `value` property and `input` event;
* **checkboxes** and **radiobuttons** use `checked` property and `change` event;
* **select** fields use `value` as a prop and `change` as an event.

**Note:** `v-model` will ignore the initial `value`, `checked`, or `selected` attributes found on any form elements. It will always treat the Vue instance data as the source of truth. You should declare the initial value on the JavaScript side, inside the `data` option of your component.

**Note:** Interpolation on textareas \(`<textarea>{{text}}</textarea>`\) won't work. Use `v-model` instead.

**Note:** If the initial value of your v-model expression does not match any of the options, the `<select>` element will render in an “unselected” state. On iOS, this will prevent the user from being able to select the first item, because iOS does not fire a `change` event in this case. It is therefore recommended to provide a `disabled` option with an empty value, as demonstrated in the example below.

```markup
<select v-model="selected">
  <option disabled value="">Please select one</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<span>Selected: {{ selected }}</span>
```

HTML:

```markup
new Vue({ el: '#app', data() { return { message: 'This is a good place to type
things.' } } });
```

Vue:

```javascript
<div id="app">
  <h3>Type here:</h3>
  <textarea v-model="message" class="message" rows="5" maxlength="72"/>
  <br>
  <p class="booktext">{{ message }} </p>
</div>
```

## V-model modifiers

**`v-model.trim`** will strip any leading or trailing whitespace from the bound string

**`v-model.number`** changes strings to number inputs

**`v-model.lazy`** won’t populate the content automatically, it will wait to bind until an event happens. \(It listens to `change` events instead of `input`\)

