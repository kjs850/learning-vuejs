# Learning Vue.js

## Overview

- Document: <https://vuejs.org/v2/guide/>

## Getting Started

add code below to `<head>`

```html
<!-- development version, includes helpful console warnings -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

or

```html
<!-- production version, optimized for size and speed -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

### Basic Syntax

- using the `Mustache` syntax (double curly braces)

```html
`<div id="app">  {{ message }}</div>`
```

- `Element` property bindings

```html
<div id="app-2">
  <span v-bind:title="message">
    Hover your mouse over me for a few seconds
    to see my dynamically bound title!
  </span>
</div>
<script>
  var app2 = new Vue({
    el: '#app-2',
    data: {
      message: 'You loaded this page on ' + new Date().toLocaleString()
    }
  })
</script>
```

- Conditionals

```html
<div id="app-3">
  <p v-if="seen">Now you see me</p>
</div>
<script>
  var app3 = new Vue({
    el: '#app-3',
    data: {
      seen: true
    }
  })
</script>
```

- Loops

```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
<script>
  var app4 = new Vue({
    el: '#app-4',
    data: {
      todos: [
        { text: 'Learn JavaScript' },
        { text: 'Learn Vue' },
        { text: 'Build something awesome' }
      ]
    }
  })
</script>
```

- Handling User Input

```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
<script>
  var app5 = new Vue({
    el: '#app-5',
    data: {
      message: 'Hello Vue.js!'
    },
    methods: {
      reverseMessage: function () {
        this.message = this.message.split('').reverse().join('')
      }
    }
  })
</script>
```

## Components

```html
<div id="app2">
  <ol>
	  <todo-item 
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id">
    </todo-item>
  </ol>
</div>
<script>
    Vue.component('todo-item', {
      template: '<li>{{todo.text}}</li>'
    });

    const app1 = new Vue({
      el: '#app2',
      data: {
        groceryList: [
          { id: 0, text: 'Vegetables' },
          { id: 1, text: 'Cheese' },
          { id: 2, text: 'Whatever else humans are supposed to eat' }
        ]
      }
    });
</script>
```



## Lifecycle

- If use data in method use `this`

```js
const app = new Vue({
  el: '#app',
  data: {
    a: 1
  },
  beforeCreate: function() { },
  created: function () { console.log('a is: ' + this.a) },
  beforeMount: function() { },
  mounted: function() { },
  beforeUpdate: : function() { },
  updated: : function() { },
  beforeDestroy: function() { },
  destroyed: function() { }
});
```

![Lifecycle](https://vuejs.org/images/lifecycle.png)





## Template Syntax

- basic form (using the `Mustache` syntext(double curly braces))

- ```html
  <span>Message: {{ msg }}</span>
  ```

-  one-time interpolations that do not update on data change by using the `v-once directive`

- ```html
  <span v-once>This will never change: {{ msg }}</span>
  ```

- Raw HTML

- ```html
  <p>Using v-html directive: <span v-html="rawHtml"></span></p>
  ```

- Using JavaScript Expressions inside bindings

- ```html
  {{ number + 1 }}
  {{ ok ? 'YES' : 'NO' }}
  {{ message.split('').reverse().join('') }}
  <div v-bind:id="'list-' + id"></div>
  ```

- The following will NOT work

- ```js
  {{ var a = 1 }}
  {{ if (ok) { return message } }}
  ```

### Shorthands

- `v-bind`

  ```html
  <a v-bind:href="url"> ... </a>
  <a :href="url"> ... </a>
  ```

- `v-on`

  ```html
  <a v-on:click="doSomething"> ... </a>
  <a @click="doSomething"> ... </a>
  ```

  

## Computed Properties and Watchers

### computed

- That’s why for any complex logic, you should use a computed property.

### computed Caching vs Method

- For computed, execute the function only when the property is changed

```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>(Computed) Reversed message: "{{ reversedMessage }}"</p>
  <p>(Mehtod) Reversed message: "{{ reversedMessageUsingMethod() }}"</p>
</div>
```

```js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    reversedMessage: function () {
      return this.message.split('').reverse().join('')
    }
  },
  method: {
    reversedMessageUsingMethod: function() {
      return this.message.split('').reverse().join('')
    }
  }
})
```

### Computed vs Watched Property

#### When Using Watch

```html
<div id="demo">{{ fullName }}</div>
<script>
  var vm = new Vue({
    el: '#demo',
    data: {
      firstName: 'Foo',
      lastName: 'Bar',
      fullName: 'Foo Bar'
    },
    watch: {
      firstName: function (val) {
        this.fullName = val + ' ' + this.lastName
      },
      lastName: function (val) {
        this.fullName = this.firstName + ' ' + val
      }
    }
  })
</script>
```

#### When Using Computed (Much better, isn’t it?)

```html
<div id="demo">{{ fullName }}</div>
<script>
  var vm = new Vue({
    el: '#demo',
    data: {
      firstName: 'Foo',
      lastName: 'Bar'
    },
    computed: {
      fullName: function () {
        return this.firstName + ' ' + this.lastName
      }
    }
  })
</script>
```

### computed setter / getter

- Computed properties are by default getter-only, but you can also provide a setter when you need it

```js
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```

- Now when you run vm.fullName = 'John Doe', the setter will be invoked and vm.firstName and vm.lastName will be updated accordingly.

### Watchers

- While computed properties are more appropriate in most cases, there are times when a custom watcher is necessary

- ```html
  <div id="watch-example">
    <p>
      Ask a yes/no question:
      <input v-model="question">
    </p>
    <p>{{ answer }}</p>
  </div>
  
  <!-- Since there is already a rich ecosystem of ajax libraries    -->
  <!-- and collections of general-purpose utility methods, Vue core -->
  <!-- is able to remain small by not reinventing them. This also   -->
  <!-- gives you the freedom to use what you're familiar with. -->
  <script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
  <script>
  var watchExampleVM = new Vue({
    el: '#watch-example',
    data: {
      question: '',
      answer: 'I cannot give you an answer until you ask a question!'
    },
    watch: {
      // whenever question changes, this function will run
      question: function (newQuestion, oldQuestion) {
        this.answer = 'Waiting for you to stop typing...'
        this.debouncedGetAnswer()
      }
    },
    created: function () {
      // _.debounce is a function provided by lodash to limit how
      // often a particularly expensive operation can be run.
      // In this case, we want to limit how often we access
      // yesno.wtf/api, waiting until the user has completely
      // finished typing before making the ajax request. To learn
      // more about the _.debounce function (and its cousin
      // _.throttle), visit: https://lodash.com/docs#debounce
      this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
    },
    methods: {
      getAnswer:  function () {
        if (this.question.indexOf('?') === -1) {
          this.answer = 'Questions usually contain a question mark. ;-)'
          return
        }
        this.answer = 'Thinking...'
        var vm = this
        axios.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
      }
    }
  })
  </script>
  ```

## Class and Style Bindings

- Syntax: `v-bind:class`

- Example: If`inActive` is only true, `class="active"`

  ```html
  <div v-bind:class="{ active: isActive }"></div>
  ```

- Example

  ```html
  <div class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
  ```

  ```json
  data: {
    isActive: true,
    hasError: false
  }
  ```


## Conditional Rendering

### v-if

```html
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```

In this case we can use v-if on a `<template>` element, which serves as an invisible wrapper

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

### v-else

```html
<div v-if="Math.random() > 0.5">Now you see me</div>
<div v-else>Now you don't</div>
```

#### v-else-if

```html
<div v-if="type === 'A'">A</div>
<div v-else-if="type === 'B'">B</div>
<div v-else-if="type === 'C'">C</div>
<div v-else>Not A/B/C</div>
```

#### v-show

- Control the style's `display` property
- It does not support the `<template>` syntax, nor does it work with ` v-else`.

#### v-if vs v-for Priority

- When used with `v-if`, `v-for` has a higher priority than `v-if`.

## List Rendering

### Basic Syntax

```html
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
<script>
  var example1 = new Vue({
    el: '#example-1',
    data: {
      items: [
        { message: 'Foo' },
        { message: 'Bar' }
      ]
    }
  })
</script>
```

### When Using Index

```html
<li v-for="(item, index) in items">
  {{ index }} - {{ item.message }}
</li>
```

### When Using Key and Index

```html
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }} : {{ value }}
</div>
```

### When Using Key

```html
<div v-for="item in items" :key="item.id">
  <!-- content -->
</div>
```

### Array Change Detection

Vue wraps an observed array’s mutation methods so they will also trigger view updates. The wrapped methods are:

- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()

### With a Range

```html
<span v-for="n in 10">
  {{ n }}
</span>
```







