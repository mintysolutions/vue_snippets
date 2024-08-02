# 1. Creating an Application

```vue
<div id="app">
  <button @click="count++">{{ count }}</button>
</div>
<script setup>
import { createApp } from 'vue';

const app = createApp({
  data() {
    return {
      count: 0
    };
  }
});

app.mount('#app');
</script>
```

```js
app.config.errorHandler = err = {};
app.component('Todo', Todo);
```

```js
const app1 = createApp({
  /* ... */
});
app1.mount('#container-1');

const app2 = createApp({
  /* ... */
});
app2.mount('#container-2');
```

# 2. Template Syntax

```js
<p>Using text interpolation: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>

```
## Attribute Bindings
```js
<div v-bind:id="dynamicId"></div>
<div :id="dynamicId"></div>

<div :id></div>
<div v-bind:id></div>

// Dynamically Binding Multiple Attributes
const objectOfAttrs = {
  id: 'container',
  class: 'wrapper',
  style: 'background-color:green'
}
<div v-bind="objectOfAttrs"></div>
```

## Using JavaScript Expressions
```js
// An expression is a piece of code that can be evaluated to a value.
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div :id="`list-${id}`"></div>

// Won't work
// this is a statement, not an expression:
{{ var a = 1 }}
// flow control won't work either, use ternary expressions
{{ if (ok) { return message } }}

<time :title="toTitleDate(date)" :datetime="date">
  {{ formatDate(date) }}
</time>

// Restricted Globals Access
const GLOBALS_ALLOWED =
  'Infinity,undefined,NaN,isFinite,isNaN,parseFloat,parseInt,decodeURI,' +
  'decodeURIComponent,encodeURI,encodeURIComponent,Math,Number,Date,Array,' +
  'Object,Boolean,String,RegExp,Map,Set,JSON,Intl,BigInt,console,Error'
```

## Directives
```js
v-html
// v-bind
<a v-bind:href="url"> ... </a>
<a :href="url"> ... </a>
// v-on
<a v-on:click="doSomething"> ... </a>
<a @click="doSomething"> ... </a>

// Dynamic Arguments
<a v-bind:[attributeName]="url"> ... </a>
<a :[attributeName]="url"> ... </a>

<a v-on:[eventName]="doSomething"> ... </a>
<a @[eventName]="doSomething"> ... </a>

// Modifiers
<form @submit.prevent="onSubmit">...</form>

```


# 3. Reactivity Fundamentals
```js
const count = ref(0)

console.log(count) // { value: 0 }
console.log(count.value) // 0

count.value++
console.log(count.value) // 1
```

```js
import { ref } from 'vue'

export default {
  setup() {
    const count = ref(0)

    function increment() {
      // .value is needed in JavaScript
      count.value++
    }

    // don't forget to expose the function as well.
    return {
      count,
      increment
    }
  }
}
```
```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

function increment() {
  count.value++
}
</script>

<template>
  <button @click="increment">
    {{ count }}
  </button>
</template>
```

## Why Refs?
```js
// In standard JavaScript, there is no way to detect the access or mutation of plain variables. 

const myRef = {
  _value: 0,
  get value() {
    track()
    return this._value
  },
  set value(newValue) {
    this._value = newValue
    trigger()
  }
}

// A ref will make its value deeply reactive.
import { ref } from 'vue'

const obj = ref({
  nested: { count: 0 },
  arr: ['foo', 'bar']
})

function mutateDeeply() {
  // these will work as expected.
  obj.value.nested.count++
  obj.value.arr.push('baz')
}
import { nextTick } from 'vue'

async function increment() {
  count.value++
  await nextTick()
  // Now the DOM is updated
}
```
```js
// Limited value types: it only works for object types (objects, arrays, and collection types such as Map and Set). It cannot hold primitive types such as string, number or boolean.


import { reactive } from 'vue'
const state = reactive({ count: 0 })
<button @click="state.count++">
  {{ state.count }}
</button>

// calling reactive() on the same object returns the same proxy
console.log(reactive(raw) === proxy) // true
// calling reactive() on a proxy returns itself
console.log(reactive(proxy) === proxy) // true

const proxy = reactive({})
const raw = {}
proxy.nested = raw
console.log(proxy.nested === raw) // false


const state = reactive({ count: 0 })
// count is disconnected from state.count when destructured.
let { count } = state
// does not affect original state
count++
// the function receives a plain number and
// won't be able to track changes to state.count
// we have to pass the entire object in to retain reactivity
callSomeFunction(state.count)


const count = ref(0)
const state = reactive({
  count
})
console.log(state.count) // 0
state.count = 1
console.log(count.value) // 1
const otherCount = ref(2)
state.count = otherCount
console.log(state.count) // 2
// original ref is now disconnected from state.count
console.log(count.value) // 1

const count = ref(0)
const object = { id: ref(1) }
{{ count + 1 }}
{{ object.id + 1 }} // not working
const { id } = object
{{ id + 1 }}

```

# 4. Computed Properties

```js
<script setup>
import { reactive, computed } from 'vue'

const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
})

// a computed ref
const publishedBooksMessage = computed(() => {
  return author.books.length > 0 ? 'Yes' : 'No'
})
</script>

<template>
  <p>Has published books:</p>
  <span>{{ publishedBooksMessage }}</span>
</template>

/**
 * Writable Computed
*/
<script setup>
import { ref, computed } from 'vue'

const firstName = ref('John')
const lastName = ref('Doe')

const fullName = computed({
  // getter
  get() {
    return firstName.value + ' ' + lastName.value
  },
  // setter
  set(newValue) {
    // Note: we are using destructuring assignment syntax here.
    [firstName.value, lastName.value] = newValue.split(' ')
  }
})
</script>
```

# 5. Class and Style Bindings

```js
const isActive = ref(true)
const hasError = ref(false)
<div
  class="static"
  :class="{ active: isActive, 'text-danger': hasError }"
></div>

// class Object
const classObject = reactive({
  active: true,
  'text-danger': false
})
const isActive = ref(true)
const error = ref(null)

const classObject = computed(() => ({
  active: isActive.value && !error.value,
  'text-danger': error.value && error.value.type === 'fatal'
}))
<div :class="classObject"></div>

// Binding to Arrays
const activeClass = ref('active')
const errorClass = ref('text-danger')
<div :class="[activeClass, errorClass]"></div>

<div :class="[isActive ? activeClass : '', errorClass]"></div>

// Component
<p class="foo bar">Hi!</p>
<MyComponent class="baz boo" />
<p class="foo bar baz boo">Hi!</p>

// multiple root elements
<p :class="$attrs.class">Hi!</p>
<span>This is a child component</span>

// Binding Inline Styles
const activeColor = ref('red')
const fontSize = ref(30)
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
<div :style="{ 'font-size': fontSize + 'px' }"></div> //  kebab-cased

const styleObject = reactive({
  color: 'red',
  fontSize: '30px'
})
<div :style="styleObject"></div>
<div :style="[baseStyles, overridingStyles]"></div>

// Multiple Values
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>


```

# 6. Conditional Rendering

```js
<button @click="awesome = !awesome">Toggle</button>
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>

<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>


<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>

<h1 v-show="ok">Hello!</h1>

```