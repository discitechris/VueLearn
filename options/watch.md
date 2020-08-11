# Watch

> **watch:** Execute code upon data changes

Type: `{ [key: string]: string | Function | Object | Array}`

Details:

An object where keys are expressions to watch and values are the corresponding callbacks. The value can also be a string of a method name, or an Object that contains additional options. The Vue instance will call `$watch()` for each entry in the object at instantiation.

Html

```markup
<div id = "app">
<button v-on:click:"counter++">Increase </button>
<button v-on:click:"counter--">Decrease </button>

<p> Counter: {{ counter }} </p>
<p> Result: {{ result() }} | {{ output }} </p>
</div>
```

Vue:

```javascript
new Vue({
    el: '#app',
    data: {
        counter: 0
    },
    computed: {
        output: function(){
            console.log('Computed');
            return this.counter > 5 ? 'Greater 5' : 'Smaller than 5';
        }
    },
    watch: {
        counter: function(value) {
            // context this.counter changes to window object in callback function below, so assiging to variable
            var vm = this;
            setTimeout(function() {
                console.log("watch");
                vm.counter = 0;

            },2000)
            // watch counter function executes at settimeout of 2s as the data value changes and assigns or clear the changed value to 0
        }

    }
    methods: {
        result: function () {
            console.log('Method');
            return this.counter > 5 ? 'Greater 5': 'Smaller than 5';

        }
    }
})
```

