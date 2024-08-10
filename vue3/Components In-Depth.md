# Component Registration
## Global Registration
```js
import { createApp } from 'vue'
const app = createApp({})
app.component(
  // the registered name
  'MyComponent',
  // the implementation
  {
    /* ... */
  }
)

import MyComponent from './App.vue'
app.component('MyComponent', MyComponent)

app
  .component('ComponentA', ComponentA)
  .component('ComponentB', ComponentB)
  .component('ComponentC', ComponentC)
```
## Local Registration
```js
<script setup>
import ComponentA from './ComponentA.vue'
</script>
<template>
  <ComponentA />
</template>


import ComponentA from './ComponentA.js'
export default {
  components: {
    ComponentA
  },
  setup() {
    // ...
  }
}
```
## Component Name Casing - PascalCase

# Props
```js
// array of strings, objects in <script setup>
defineProps(['title', 'likes'])
defineProps({
  title: String,
  likes: Number
})

// in non-<script setup>
export default {
  props: ['foo'],
  setup(props) {
    // setup() receives props as the first argument.
    console.log(props.foo)
  }
}
export default {
  props: {
    title: String,
    likes: Number
  }
}
```

## Binding Multiple Properties Using an Object
```js
const post = {
  id: 1,
  title: 'My Journey with Vue'
}
<BlogPost v-bind="post" />
<BlogPost :id="post.id" :title="post.title" />
```

## One-Way Data Flow

In most cases, the child should emit an event to let the parent perform the mutation.

```js
const props = defineProps(['initialCounter'])
// counter only uses props.initialCounter as the initial value;
// it is disconnected from future prop updates.
const counter = ref(props.initialCounter)
```
```js
const props = defineProps(['size'])
// computed property that auto-updates when the prop changes
const normalizedSize = computed(() => props.size.trim().toLowerCase())
```
## Prop Validation
optional prop other than Boolean will have undefined value.
```js
defineProps({
  // Basic type check
  //  (`null` and `undefined` values will allow any type)
  propA: Number,
  // Multiple possible types
  propB: [String, Number],
  // Required string
  propC: {
    type: String,
    required: true
  },
  // Required but nullable string
  propD: {
    type: [String, null],
    required: true
  },
  // Number with a default value
  propE: {
    type: Number,
    default: 100
  },
  // Object with a default value
  propF: {
    type: Object,
    // Object or array defaults must be returned from
    // a factory function. The function receives the raw
    // props received by the component as the argument.
    default(rawProps) {
      return { message: 'hello' }
    }
  },
  // Custom validator function
  // full props passed as 2nd argument in 3.4+
  propG: {
    validator(value, props) {
      // The value must match one of these strings
      return ['success', 'warning', 'danger'].includes(value)
    }
  },
  // Function with a default value
  propH: {
    type: Function,
    // Unlike object or array default, this is not a factory
    // function - this is a function to serve as a default value
    default() {
      return 'Default function'
    }
  }
})
```
## Boolean casting
```js
// disabled will be casted to true
defineProps({
  disabled: [Boolean, Number]
})

// disabled will be casted to true
defineProps({
  disabled: [Boolean, String]
})

// disabled will be casted to true
defineProps({
  disabled: [Number, Boolean]
})

// disabled will be parsed as an empty string (disabled="")
defineProps({
  disabled: [String, Boolean]
})
```