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

# 7. List Rendering
```js
// indexed
const items = ref([{ message: 'Foo' }, { message: 'Bar' }])
<li v-for="(item, index) in items">
  {{ parentMessage }} - {{ index }} - {{ item.message }}
</li>
<div v-for="item of items"></div>

// object
<li v-for="(value, key, index) in myObject">
  {{ index }}. {{ key }}: {{ value }}
</li>

<span v-for="n in 10">{{ n }}</span>

// v-for with v-if
// !!! error!!!
// When they exist on the same node, v-if has a higher priority than v-for. That means the v-if condition will not have access to variables from the scope of the v-for:
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo.name }}
</li>
// Working!
<template v-for="todo in todos">
  <li v-if="!todo.isComplete">
    {{ todo.name }}
  </li>
</template>

<MyComponent
  v-for="(item, index) in items"
  :item="item"
  :index="index"
  :key="item.id"
/>

// Mutation Methods​
// Vue is able to detect when a reactive array's mutation methods are called and trigger necessary updates. These mutation methods are:

// push()
// pop()
// shift()
// unshift()
// splice()
// sort()
// reverse()
// `items` is a ref with array value

// non-mutating methods, e.g. filter(), concat() and slice()
// Vue doesn't re-render the entire list
items.value = items.value.filter((item) => item.message.match(/Foo/))


const numbers = ref([1, 2, 3, 4, 5])
const evenNumbers = computed(() => {
  return numbers.value.filter((n) => n % 2 === 0)
})
const sortedNumbers = computed(()=>{
  // - return numbers.reverse()
  return [...numbers].reverse()
  return [...numbers].sort()
})

const sets = ref([
  [1, 2, 3, 4, 5],
  [6, 7, 8, 9, 10]
])
function even(numbers) {
  return numbers.filter((number) => number % 2 === 0)
}
<ul v-for="numbers in sets">
  <li v-for="n in even(numbers)">{{ n }}</li>
</ul>

```

# 8. Event Handling

```js
<button @click="count++">Add 1</button>
<p>Count is: {{ count }}</p>

function say(message) {
  alert(message)
}
<button @click="say('hello')">Say hello</button>
<button @click="say('bye')">Say bye</button>

/**
 * Accessing Event Argument in Inline Handlers
*/
// <!-- using $event special variable -->
<button @click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
// <!-- using inline arrow function -->
function warn(message, event) {
  if (event) {
    event.preventDefault()
  }
  alert(message)
}
<button @click="(event) => warn('Form cannot be submitted yet.', event)">
  Submit
</button>

/**
 * Event Modifiers
 */

// .stop
// .prevent
// .self
// .capture
// .once
// .passive

// <!-- the click event's propagation will be stopped -->
<a @click.stop="doThis"></a>
// <!-- the submit event will no longer reload the page -->
<form @submit.prevent="onSubmit"></form>
// <!-- modifiers can be chained -->
<a @click.stop.prevent="doThat"></a>
// <!-- just the modifier -->
<form @submit.prevent></form>
// <!-- only trigger handler if event.target is the element itself -->
// <!-- i.e. not from a child element -->
<div @click.self="doThat">...</div>

/**
 * Key Modifiers
 */
// .enter
// .tab
// .delete (captures both "Delete" and "Backspace" keys)
// .esc
// .space
// .up
// .down
// .left
// .right
// <!-- only call `submit` when the `key` is `Enter` -->
<input @keyup.enter="submit" />
<input @keyup.page-down="onPageDown" />

/**
 * System Modifier Keys
 */
// .ctrl
// .alt
// .shift
// .meta

// <!-- Alt + Enter -->
<input @keyup.alt.enter="clear" />
// <!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>

// .exact
// <!-- this will fire even if Alt or Shift is also pressed -->
<button @click.ctrl="onClick">A</button>
// <!-- this will only fire when Ctrl and no other keys are pressed -->
<button @click.ctrl.exact="onCtrlClick">A</button>
// <!-- this will only fire when no system modifiers are pressed -->
<button @click.exact="onClick">A</button>

/**
 * Mouse Button Modifiers
 */
// .left
// .right
// .middle

```

# 9.Form Input Bindings
```js
// v-model
<input
  :value="text"
  @input="event => text = event.target.value">
<input v-model="text">
```
```js
<label for="cars">Choose a car:</label>
<select name="cars" id="cars">
  <optgroup label="Swedish Cars">
    <option value="volvo">Volvo</option>
    <option value="saab">Saab</option>
  </optgroup>
  <optgroup label="German Cars">
    <option value="mercedes">Mercedes</option>
    <option value="audi">Audi</option>
  </optgroup>
</select>

<textarea v-model="message" placeholder="add multiple lines"></textarea>
```
```js

// vehicle1=Bike&vehicle2=Car&vehicle3=Boat 
<input type="checkbox" id="vehicle1" name="vehicle1" value="Bike">
<label for="vehicle1"> I have a bike</label><br>
<input type="checkbox" id="vehicle2" name="vehicle2" value="Car">
<label for="vehicle2"> I have a car</label><br>
<input type="checkbox" id="vehicle3" name="vehicle3" value="Boat">
<label for="vehicle3"> I have a boat</label><br><br>

<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no" />

//  bind multiple checkboxes to the same array or Set value:
<div>Checked names: {{ checkedNames }}</div>
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames" />
<label for="jack">Jack</label>
<input type="checkbox" id="john" value="John" v-model="checkedNames" />
<label for="john">John</label>
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames" />
<label for="mike">Mike</label>
```
```js
<input type="radio" id="html" name="fav_language" value="HTML">
<label for="html">HTML</label><br>
<input type="radio" id="css" name="fav_language" value="CSS">
<label for="css">CSS</label><br>
<input type="radio" id="javascript" name="fav_language" value="JavaScript">
<label for="javascript">JavaScript</label>

<div>Picked: {{ picked }}</div>
<input type="radio" id="one" value="One" v-model="picked" />
<label for="one">One</label>
<input type="radio" id="two" value="Two" v-model="picked" />
<label for="two">Two</label>
```
```js
<div>Selected: {{ selected }}</div>
<select v-model="selected" multiple>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>

const selected = ref('A')
const options = ref([
  { text: 'One', value: 'A' },
  { text: 'Two', value: 'B' },
  { text: 'Three', value: 'C' }
])
<select v-model="selected">
  <option v-for="option in options" :value="option.value">
    {{ option.text }}
  </option>
</select>
<div>Selected: {{ selected }}</div>
```

```js
// <!-- when the input loses focus or when the user presses Enter. -->
// synced after "change" instead of "input"
<input v-model.lazy="msg" />
// typecast as a number
<input v-model.number="age" />

<input v-model.trim="msg" />
```

# 10.Lifecycle Hooks

```js
function onMounted(callback: () => void): void
function onUpdated(callback: () => void): void

```

# 11.Watchers
```js
// it can be a 
// ref (including computed refs), 
// a reactive object, 
// a getter function, 
// or an array of multiple sources:
const x = ref(0)
const y = ref(0)

// single ref
watch(x, (newX) => {
  console.log(`x is ${newX}`)
})

// getter
watch(
  () => x.value + y.value,
  (sum) => {
    console.log(`sum of x + y is: ${sum}`)
  }
)

// array of multiple sources
watch([x, () => y.value], ([newX, newY]) => {
  console.log(`x is ${newX} and y is ${newY}`)
})
</script>

<template>
  <p>
    Ask a yes/no question:
    <input v-model="question" :disabled="loading" />
  </p>
  <p>{{ answer }}</p>
</template>
```

```js
const obj = reactive({ count: 0 })
// this won't work because we are passing a number to watch()
watch(obj.count, (count) => {
  console.log(`Count is: ${count}`)
})
// instead, use a getter:
watch(
  () => obj.count,
  (count) => {
    console.log(`Count is: ${count}`)
  }
)
```
## Deep Watchers
```js
// implicitly create a deep
const obj = reactive({ count: 0 })
watch(obj, (newValue, oldValue) => {
  // fires on nested property mutations
  // Note: `newValue` will be equal to `oldValue` here
  // because they both point to the same object!
})
obj.count++


watch(
  () => state.someObject,
  () => {
    // fires only when state.someObject is replaced
  }
)

watch(
  () => state.someObject,
  (newValue, oldValue) => {
    // Note: `newValue` will be equal to `oldValue` here
    // *unless* state.someObject has been replaced
  },
  { deep: true }
)
```


```js
//  Eager Watchers
// watch is lazy by default: the callback won't be called until the watched source
watch(
  source,
  (newValue, oldValue) => {
    // executed immediately, then again when `source` changes
  },
  { immediate: true }
)

// Once Watchers 
watch(
  source,
  (newValue, oldValue) => {
    // when `source` changes, triggers only once
  },
  { once: true }
)

```
## watchEffect()
```js
const todoId = ref(1)
const data = ref(null)

watch(
  todoId,
  async () => {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
    )
    data.value = await response.json()
  },
  { immediate: true }
)
watchEffect(async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  data.value = await response.json()
})
```
## Post and Sync Watcher
```js
// Post Watchers
// If you want to access the owner component's DOM in a watcher callback after Vue has updated it
watch(source, callback, {
  flush: 'post'
})
watchEffect(callback, {
  flush: 'post'
})
import { watchPostEffect } from 'vue'
watchPostEffect(() => {
  /* executed after Vue updates */
})

// Sync Watchers
// It's also possible to create a watcher that fires synchronously,
watch(source, callback, {
  flush: 'sync'
})

watchEffect(callback, {
  flush: 'sync'
})
import { watchSyncEffect } from 'vue'

watchSyncEffect(() => {
  /* executed synchronously upon reactive data change */
})
```
## Stopping a Watcher
```js
// this one will be automatically stopped
watchEffect(() => {})
// ...this one will not!
setTimeout(() => {
  watchEffect(() => {})
}, 100)


const unwatch = watchEffect(() => {})
// ...later, when no longer needed
unwatch()
```

# 12. Template Refs

```js
import { ref, onMounted } from 'vue'
// declare a ref to hold the element reference
// the name must match template ref value
const input = ref(null)

onMounted(() => {
  input.value.focus()
})
</script>

<template>
  <input ref="input" />
</template>

```

```js
import { ref, onMounted } from 'vue'

const list = ref([
  /* ... */
])

const itemRefs = ref([])

onMounted(() => console.log(itemRefs.value))
</script>

<template>
  <ul>
    <li v-for="item in list" ref="itemRefs">
      {{ item }}
    </li>
  </ul>
</template>
```
```js
import { ref, onMounted } from 'vue'
import Child from './Child.vue'

const child = ref(null)

onMounted(() => {
  // child.value will hold an instance of <Child />
})
</script>

<template>
  <Child ref="child" />
</template>
```