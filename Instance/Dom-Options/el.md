> **el:** connect to DOM

Type: `string` | `Element`

Restriction: only respected in instance creation via `new`.

Details:

Provide the Vue instance an existing DOM element to mount on. It can be a CSS selector string or an actual HTMLElement.

After the instance is mounted, the resolved element will be accessible as `vm.$el`.

If this option is available at instantiation, the instance will immediately enter compilation; otherwise, the user will have to explicitly call `vm.$mount()` to manually start the compilation.

**Warning**:
The provided element merely serves as a mounting point. Unlike in Vue 1.x, the mounted element will be replaced with Vue-generated DOM in all cases. It is therefore not recommended to mount the root instance to `<html>` or `<body>`.

**Warning**:
If neither `render` function nor `template` option is present, the in-DOM HTML of the mounting DOM element will be extracted as the template. In this case, Runtime + Compiler build of Vue should be used.
