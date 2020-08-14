# v-slot \(or\) \#

Denote named slots or slots that expect to receive props.

**Expects:** JavaScript expression that is valid in a function argument position \(supports destructuring in supported environments\). Optional - only needed if expecting props to be passed to the slot.

**Argument:** slot name \(optional, defaults to `default`\)

Limited to:

* `<template>`
* components \(for a lone default slot with props\)

Example:

```markup
<!-- Named slots -->
<base-layout>
  <template v-slot:header>
    Header content
  </template>

  Default slot content

  <template v-slot:footer>
    Footer content
  </template>
</base-layout>

<!-- Named slot that receives props -->
<infinite-scroll>
  <template v-slot:item="slotProps">
    <div class="item">
      {{ slotProps.item.text }}
    </div>
  </template>
</infinite-scroll>

<!-- Default slot that receive props, with destructuring -->
<mouse-position v-slot="{ x, y }">
  Mouse position: {{ x }}, {{ y }}
</mouse-position>
```

