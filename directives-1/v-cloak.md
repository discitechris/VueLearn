# v-cloak

This property remains on the element until the associated ViewModel finishes compilation. Combined with CSS rules such as **`[v-cloak] { display: none }`**, this directive can be used to hide un-compiled mustache bindings until the ViewModel is ready.

CSS:

```css
[v-cloak] {
  display: none;
}
```

The `<div>` will not be visible until the compilation is done.

Html:

```markup
<div v-cloak>
  {{ message }}
</div>
```

