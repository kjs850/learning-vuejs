# Vue.js 를 배워 보자

## 개요

- 문서: <https://kr.vuejs.org/v2/guide/>

## 시작

`<head>`에 아래와 같이 추가

```html
<!-- 도움되는 콘솔 경고를 포함한 개발 버전 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

또는

```html
!-- 상용버전, 속도와 용량이 최적화됨. -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

### 기본적인 문법

- `Mustache` 문법을 사용. (중괄호 2개)

```html
`<div id="app">  {{ message }}</div>`
```

- `Element` 속성 바인딩

```html
<div id="app-2">
  <span v-bind:title="message">
    내 위에 잠시 마우스를 올리면 동적으로 바인딩 된 title을 볼 수 있습니다!
  </span>
</div>
<script>
  var app2 = new Vue({
    el: '#app-2',
    data: {
      message: '이 페이지는 ' + new Date() + ' 에 로드 되었습니다'
    }
  })
</script>
```

- 조건문

```html
<div id="app-3">
  <p v-if="seen">이제 나를 볼 수 있어요</p>
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

- 반복문

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
        { text: 'JavaScript 배우기' },
        { text: 'Vue 배우기' },
        { text: '무언가 멋진 것을 만들기' }
      ]
    }
  })
</script>
```

- 사용자 입력 핸들링

```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">메시지 뒤집기</button>
</div>
<script>
  var app5 = new Vue({
    el: '#app-5',
    data: {
      message: '안녕하세요! Vue.js!'
    },
    methods: {
      reverseMessage: function () {
        this.message = this.message.split('').reverse().join('')
      }
    }
  })
</script>
```

## 콤포넌트

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



## 라이프사이클

- method 안에서 data 를 사용할 경우 `this` 를 활용

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





## 템플릿 문법

- `Mustache` 문법을 사용 (중괄호 2개)

- ```html
  <span>Message: {{ msg }}</span>
  ```

- 데이터 변경 시 업데이트 되지 않고 일회성으로만 보여 줄 경우 `v-once` 를 사용

- ```html
  <span v-once>This will never change: {{ msg }}</span>
  ```

- HTML 을 그대로 이용

- ```html
  <p>Using v-html directive: <span v-html="rawHtml"></span></p>
  ```

- 바인딩 내에서 JavaScript 표현식 사용

- ```html
  {{ number + 1 }}
  {{ ok ? 'YES' : 'NO' }}
  {{ message.split('').reverse().join('') }}
  <div v-bind:id="'list-' + id"></div>
  ```

- 아래 처럼 작성하면 안됨

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

  

## Computed 와 Watchers

### computed

- 복잡한 로직일 수록 html내의 Template 내에서 구현하기 보다는 computed 를 활용할 것 (유지 보수 등을 위해)

### computed Caching vs Method

- computed 의 경우 해당 속성이 변경될  때만 함수를 실행
- 즉, 아래 예제에서 message 값이 변경되어야 반영됨

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

### Computed 와 Watched 의 속성 (Property)

#### watch 를 사용했을 경우

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

#### computed 를 사용했을 경우 (이게 더 낫지 않나?)

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

- Computed 속성은 기본적으로 getter 함수만 가지고 있지만,
- 필요한 경우 setter 함수를 만들어 쓸 수 있다.

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

- `vm.fullName = 'John Doe'`를 실행하면 설정자가 호출되고 `vm.firstName`과 `vm.lastName`이 그에 따라 업데이트 됩니다.

### Watchers

- 대부분의 경우 computed 가 적합

- 하지만 사용자가 만든 `Watcher`가 필요한 경우가 있음

- ```html
  <div id="watch-example">
    <p>
      yes/no 질문을 물어보세요:
      <input v-model="question">
    </p>
    <p>{{ answer }}</p>
  </div>
  
  <!-- 이미 Ajax 라이브러리의 풍부한 생태계와 범용 유틸리티 메소드 컬렉션이 있기 때문에, -->
  <!-- Vue 코어는 다시 만들지 않아 작게 유지됩니다. -->
  <!-- 이것은 이미 익숙한 것을 선택할 수 있는 자유를 줍니다. -->
  <script src="https://unpkg.com/axios@0.12.0/dist/axios.min.js"></script>
  <script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>
  <script>
  var watchExampleVM = new Vue({
    el: '#watch-example',
    data: {
      question: '',
      answer: '질문을 하기 전까지는 대답할 수 없습니다.'
    },
    watch: {
      // 질문이 변경될 때 마다 이 기능이 실행됩니다.
      question: function (newQuestion) {
        this.answer = '입력을 기다리는 중...'
        this.getAnswer()
      }
    },
    methods: {
      // _.debounce는 lodash가 제공하는 기능으로
      // 특히 시간이 많이 소요되는 작업을 실행할 수 있는 빈도를 제한합니다.
      // 이 경우, 우리는 yesno.wtf/api 에 액세스 하는 빈도를 제한하고,
      // 사용자가 ajax요청을 하기 전에 타이핑을 완전히 마칠 때까지 기다리길 바랍니다.
      // _.debounce 함수(또는 이와 유사한 _.throttle)에 대한
      // 자세한 내용을 보려면 https://lodash.com/docs#debounce 를 방문하세요.
      getAnswer: _.debounce(
        function () {
          if (this.question.indexOf('?') === -1) {
            this.answer = '질문에는 일반적으로 물음표가 포함 됩니다. ;-)'
            return
          }
          this.answer = '생각중...'
          var vm = this
          axios.get('https://yesno.wtf/api')
            .then(function (response) {
              vm.answer = _.capitalize(response.data.answer)
            })
            .catch(function (error) {
              vm.answer = '에러! API 요청에 오류가 있습니다. ' + error
            })
        },
        // 사용자가 입력을 기다리는 시간(밀리세컨드) 입니다.
        500
      )
    }
  })
  </script>
  ```

## Class 와 Style 바인딩

- 사용 형태: `v-bind:class`

- 예제: `inActive` 가 true 경우에만 `class="active"`

  ```html
  <div v-bind:class="{ active: isActive }"></div>
  ```

- 예제를 더 보자

  ```html
  <div class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
  ```

  ```json
  data: {
    isActive: true,
    hasError: false
  }
  ```

## 조건문 렌더링

### v-if

```html
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```

보이지 않은 레퍼 역할로 사용해야 할 경우 `<template>` 사용

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

- Style의 `display` 속성을 컨트롤
- `<template>` 구문을 지원 않으며, `v-else` 와도 작동하지 않는다.

#### v-if 와 v-for 우선순위

- `v-if`오ㅏ 함께 사용하는 경우, `v-for` 는 `v-if`보다 높은 우선 순위를 갖는다.



## 리스트 렌더링

### 기본 사용법

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

### index 사용 시

```html
<li v-for="(item, index) in items">
  {{ index }} - {{ item.message }}
</li>
```

### Key, Index 사용 시

```html
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }} : {{ value }}
</div>
```

### Key 활용

```html
<div v-for="item in items" :key="item.id">
  <!-- content -->
</div>
```

### 배열 변경 감지

아래 메소드를 통하여 변경이 이루어 졌을 때 뷰 갱신한다.

- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()

### 숫자 활용

```html
<span v-for="n in 10">
  {{ n }}
</span>
```







