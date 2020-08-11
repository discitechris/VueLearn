# Components

Components are reusable Vue instances with a name: in this case, `<button-counter>`. We can use this component as a custom element inside a root Vue instance created with new Vue:

Vue:

```js
new Vue({ el: "#components-demo" });

// Define a new component called button-counter
Vue.component("button-counter", {
  data: function () {
    return {
      count: 0,
    };
  },
  template:
    '<button v-on:click="count++"> You clicked me {{count}} times. </button>',
});
```

Html:

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

Since components are reusable Vue instances, they accept the same options as `new Vue`, such as `data`, `computed`, `watch`, `methods`, and lifecycle hooks. The only exceptions are a few root specific options like `el`.

## Reusing Components

Components can be reused as many times as you want:

Html:

```html
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

Notice that when clicking on the buttons, each one maintains its own, separate `count`. That's because each time you use a component, a new **instance** of it is created.

## `data` Must be a function

When we defined the `<button-counter>` component, you may have noticed that `<data>` wasn't directly provided an object, like this:

Vue:

```js
data: {
  count: 0;
}
```

Instead, a **component’s `data` option must be a function**, so that each instance can maintain an independent copy of the returned data object:

Vue:

```js
data: function () {
  return {
    count: 0
  }
}
```

If Vue didn’t have this rule, clicking on one button would affect the data of _all other instances_,

## Organizing Components

It’s common for an app to be organized into a tree of nested components:

![image](../components/components.png)

For example, you might have components for a header, sidebar, and content area, each typically containing other components for navigation links, blog posts, etc.

- To use these components in templates, they must be registered so that Vue knows about them.
- There are two types of component registration: **global** and **local**.

So far, we’ve only registered components globally, using `Vue.component`

Vue:

```js
Vue.component("my-component-name", {
  // ... options ...
});
```

## Passing Data to Child Components with Props

Props are custom attributes you can register on a component. When a value is passed to a prop attribute, it becomes a property on that component instance. To pass a title to our blog post component, we can include it in the list of props this component accepts, using a `props` option:

Vue:

```js
Vue.component("blog-post", {
  props: ["title"],
  template: "<h3>{{ title }}</h3>",
});
```

A component can have as many props as you’d like and by default, any value can be passed to any prop. In the template above, you’ll see that we can access this value on the component instance, just like with `data`.

Once a prop is registered, you can pass data to it as a custom attribute, like this:

Html:

```html
<blog-post title="My journey with Vue"></blog-post>
<blog-post title="Blogging with Vue"></blog-post>
<blog-post title="Why Vue is so fun"></blog-post>
```

In a typical app, however, you’ll likely have an array of posts in `data`:

Vue:

```js
new Vue({
  el: "#blog-post-demo",
  data: {
    posts: [
      { id: 1, title: "My journey with Vue" },
      { id: 2, title: "Blogging with Vue" },
      { id: 3, title: "Why Vue is so fun" },
    ],
  },
});
```

Then want to render a component for each one:

```html
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
></blog-post>
```

Above, you’ll see that we can use `v-bind` to dynamically pass props. This is especially useful when you don’t know the exact content you’re going to render ahead of time, like when fetching posts from an API.

When building out a `<blog-post>` component, your template will eventually contain more than just the title:

```html
<h3>{{ title }}</h3>
<!-- At the very least, you’ll want to include the post’s content: -->

<h3>{{ title }}</h3>
<div v-html="content"></div>
```

If you try this in your template however, Vue will show an error, explaining that **every component must have a single root element**. You can fix this error by wrapping the template in a parent element, such as:

```html
<div class="blog-post">
  <h3>{{ title }}</h3>
  <div v-html="content"></div>
</div>
```

As our component grows, it’s likely we’ll not only need the title and content of a post, but also the published date, comments, and more. Defining a prop for each related piece of information could become very annoying:

```html
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
  v-bind:content="post.content"
  v-bind:publishedAt="post.publishedAt"
  v-bind:comments="post.comments"
></blog-post>
```

## A Single Root Element

So this might be a good time to refactor the `<blog-post>` component to accept a single post prop instead:

Html:

```html
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:post="post"
></blog-post>
```

Vue:

```js
Vue.component("blog-post", {
  props: ["post"],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <div v-html="post.content"></div>
    </div>
  `,
});
```

**Note:**
The above example and some future ones use JavaScript’s template literal to make multi-line templates more readable.

# Listening to Child Components Events

As we develop our `<blog-post>` component, some features may require communicating back up to the parent. For example, we may decide to include an accessibility feature to enlarge the text of blog posts, while leaving the rest of the page its default size:

In the parent, we can support this feature by adding a `postFontSize` data property:

parent instance/component Vue:

```js
new Vue({
  el: "#blog-posts-events-demo",
  data: {
    posts: [
      /* ... */
    ],
    postFontSize: 1,
  },
});
```

Which can be used in the template to control the font size of all blog posts:

```html
<div id="blog-posts-events-demo">
  <div :style="{ fontSize: postFontSize + 'em' }">
    <blog-post
      v-for="post in posts"
      v-bind:key="post.id"
      v-bind:post="post"
    ></blog-post>
  </div>
</div>
```

Now let’s add a button to enlarge the text right before the content of every post:

child Component Vue:

```js
Vue.component("blog-post", {
  props: ["post"],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <button>
        Enlarge text
      </button>
      <div v-html="post.content"></div>
    </div>
  `,
});
```

When we click on the button, we need to communicate to the parent that it should enlarge the text of all posts. Fortunately, Vue instances provide a custom events system to solve this problem. The parent can choose to listen to any event on the child component instance with `v-on`, just as we would with a native DOM event.

Then the child component can emit an event on itself by calling the built-in **`$emit`** **method**, passing the name of the event:

html:

```html
<div id="blog-posts-events-demo">
  <div :style="{ fontSize: postFontSize + 'em' }">
    <blog-post
      v-for="post in posts"
      v-bind:key="post.id"
      v-bind:post="post"
      v-on:enlarge-text="postFontSize += 0.1"
    ></blog-post>
  </div>
</div>
```

Vue:

```js

new Vue({
  el: "#blog-posts-events-demo",
  data: {
    posts: [
      /* ... */
    ],
    postFontSize: 1,
  },
});

Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
        <button v-on:click="$emit('enlarge-text')">
        Enlarge text
        </button>
      <div v-html="post.content"></div>
    </div>
  `

```

# Emitting a Value With an Event

It’s sometimes useful to emit a specific value with an event. For example, we may want the `<blog-post>` component to be in charge of how much to enlarge the text by. In those cases, we can use `$emit`‘s 2nd parameter to provide this value:

```js

new Vue({
  el: "#blog-posts-events-demo",
  data: {
    posts: [
      /* ... */
    ],
    postFontSize: 1,
  },
});
        // below using $emit‘s 2nd parameter
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>

        <button v-on:click="$emit('enlarge-text', 0.1 )">
        Enlarge text
        </button>
      <div v-html="post.content"></div>
    </div>
  `

```

html:

```html
<!--
      Then
      when
      we
      listen
      to
      the
      event
      in
      the
      parent,
      we
      can
      access
      the
      emitted
      event’s
      value
      with
      $event:
      -->
<div id="blog-posts-events-demo">
  <div :style="{ fontSize: postFontSize + 'em' }">
    <blog-post
      v-for="post in posts"
      v-bind:key="post.id"
      v-bind:post="post"
      v-on:enlarge-text="postFontSize +=$event"
    ></blog-post>
  </div>
</div>
```

Or, if the event handler is a method:

```js

new Vue({
  el: "#blog-posts-events-demo",
  data: {
    posts: [
      /* ... */
    ],
    postFontSize: 1,
  },
  methods: {
  onEnlargeText: function (enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
},
});
        // below using $emit‘s 2nd parameter
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
        <button v-on:click="$emit('enlarge-text', 0.1 )">
        Enlarge text
        </button>
      <div v-html="post.content"></div>
    </div>
  `

```

html:

```html
<!--if
      the
      event
      handler
      is
      a
      method-->
<div id="blog-posts-events-demo">
  <div :style="{ fontSize: postFontSize + 'em' }">
    <blog-post
      v-for="post in posts"
      v-bind:key="post.id"
      v-bind:post="post"
      v-on:enlarge-text="onEnlargeText"
    ></blog-post>
  </div>
</div>
```

# Using v-model on Components

Custom events can also be used to create custom inputs that work with `v-model`. Remember that:

```html
<input v-model="searchText" />
```

does the same thing as:

```html
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
/>
```

When used on a component, `v-model` instead does this:

```html
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>
```

For this to actually work though, the `<input>` inside the component must:

- Bind the `value` attribute to a `value` prop
- On `input`, emit its own custom `input` event with the new value

Here's that in action:

```js
Vue.component("custom-input", {
  props: ["value"],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `,
});
```

Now `v-model` should work perfectly with this component:

```html
<custom-input v-model="searchText"></custom-input>
```

# Content Distribution with Slots

Just like with HTML elements, it’s often useful to be able to pass content to a component, like this:

```html
<alert-box>
  Something bad happened
</alert-box>
```

Which might render something like this:
![image](slotoutput.jpg)

Fortunately, this task is made very simple by Vue’s custom `<slot>` element:

```js
Vue.component("alert-box", {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `,
});
```

# Dynamic Components

Sometimes, it’s useful to dynamically switch between components, like in a tabbed interface:
![image](dynamiccomp.jpg)

The above is made possible by Vue’s <component> element with the **`is`** special attribute:

```html
<!-- Component changes when currentTabComponent changes -->
<component v-bind:is="currentTabComponent"></component>
```

In the example above, `currentTabComponent` can contain either:

- the name of a registered component, or
- a component's options object

See [this example](https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-dynamic-components) to experiment with the full code, or [this version](https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-dynamic-components-with-binding) for an example binding to a component’s options object, instead of its registered name.

Keep in mind that this attribute can be used with regular HTML elements, however they will be treated as components, which means all attributes **will be bound as DOM attributes**. For some properties such as `value` to work as you would expect, you will need to bind them using the **.prop modifier**.

# DOM Template Parsing Caveats

Some HTML elements, such as `<ul>`, `<ol>`, `<table>` and `<select>` have restrictions on what elements can appear inside them, and some elements such as `<li>`, `<tr>`, and `<option>` can only appear inside certain other elements.

This will lead to issues when using components with elements that have such restrictions. For example:

```html
<table>
  <blog-post-row></blog-post-row>
</table>
```

The custom component `<blog-post-row>` will be hoisted out as invalid content, causing errors in the eventual rendered output. Fortunately, the **`is`** special attribute offers a workaround:

```html
<table>
  <tr is="blog-post-row"></tr>
</table>
```

It should be noted that **this limitation does _not_ apply if you are using string templates from one of the following sources**:

- String templates (e.g. `template: '...'`)
- Single-file (.vue) components
- `<script type="text/x-template">`
