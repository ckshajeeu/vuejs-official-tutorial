<template>
  <div style="text-align: center;margin-bottom: 25px;">
    <h1>Vue.js Tutorial</h1>
    <h4><a href="https://vuejs.org/tutorial/#step-1" target="_blank">Link to official tutorial</a> (Composition, SFC)</h4>
  </div>

  <div class="container">
    <div class="topic">Getting Started</div>
    <div><h1>{{ message }}</h1></div>
  </div>
  
  <div class="container">
    <div class="topic">Declarative Rendering</div>
    <div>
      <button>{{ counter.count }}</button>
    </div>
  </div>
  <br/>

  <div class="container">
    <div class="topic">Attribute Bindings</div>
    <div>
      <div><h1 :class="titleClass">Make me red</h1></div>
    </div>
  </div>

  <div class="container">
    <div class="topic">Event Listeners</div>
    <div>
      <button v-on:click="increment">{{ counter2 }}</button>
    </div>
  </div>
  <br/>

  <div class="container">
    <div class="topic">Form Bindings</div>
    <div>
      <input :value="text" @input="onInput" placeholder="Type here"></input> &nbsp;
      {{ text }}
    </div>
  </div>

  <div class="container">
    <div class="topic">Conditional Rendering</div>
    <div>
      <button @click="toggle">Toggle</button> &nbsp
      <span v-if="awesome">Vue is awesome!</span>
      <span v-else>Oh no!!!</span>
    </div>
  </div>

  <div class="container">
    <div class="topic">List Rendering</div>
    <div>
      <form @submit.prevent="addTodo">
        <input v-model="newTodo" required placeholder="new todo" /> &nbsp;
        <button>Add Todo</button>
      </form>
      <ul>
        <li v-for="todo in todos" :key="todo.id">
          {{ todo.text }}
          <button @click="removeTodo(todo)">X</button>
        </li>
      </ul>
    </div>
  </div>

  <div class="container">
    <div class="topic">Computed Property</div>
    <div>
      <form @submit.prevent="addTodo">
        <input v-model="newTodo" required placeholder="new todo" /> &nbsp;
        <button>Add Todo</button>
      </form>
      <ul>
        <li v-for="todo in filteredTodos" :key="todo.id">
        <input type="checkbox" v-model="todo.done"></input>
          {{ todo.text }}
          <button @click="removeTodo(todo)">X</button>
        </li>
      </ul>
      <button @click="hideCompleted = !hideCompleted">Hide completed</button>
    </div>
  </div>

  <div class="container">
    <div class="topic">Lifecycle and Template Refs</div>
    <div>
      <p ref="pElementRef">Hello</p>
    </div>
  </div>

  <div class="container">
    <div class="topic">Watchers</div>
    <div>
      <p>Todo id: {{ todoId }}</p>
      <button @click="todoId++" :disabled="!todoData">Fetch next todo</button>
      <p v-if="!todoData">Loading...</p>
      <p v-else>{{ todoData }}</p>
    </div>
  </div>

  <div class="container">
    <div class="topic">Components</div>
    <div>
      <ChildComp1 />
    </div>
  </div>

  <div class="container">
    <div class="topic">Props</div>
    <div>
      <ChildComp2 :msg="greeting"/>
    </div>
  </div>

  <div class="container">
    <div class="topic">Emits</div>
    <div>
      <ChildComp3 @response="(msg) => childMsg = msg"/>
      <p>{{ childMsg }}</p>
    </div>
  </div>

  <div class="container">
    <div class="topic">Slots</div>
    <div>
      <ChildComp4>Message : {{ msg }}</ChildComp4>
    </div>
  </div>
  
</template>


<script setup lang="ts">
  import { computed, onMounted, reactive, ref, watch } from 'vue'
  import ChildComp1 from './components/ChildComp1.vue'
  import ChildComp2 from './components/ChildComp2.vue'
  import ChildComp3 from './components/ChildComp3.vue'
  import ChildComp4 from './components/ChildComp4.vue'
  
  const counter = reactive({
    count: 0
  })
  const message = ref('Hello world!')
  
  const counter2 = ref(0)
  function increment() {
    counter2.value += 1
  }

  const titleClass = ref('title')

  const text = ref('')
  function onInput(e: any) {
    text.value = e.target.value
  }

  const awesome = ref(true)
  function toggle(e:any) {
    awesome.value = !awesome.value
  }

  let id = 0
  const newTodo =  ref('')
  const todos = ref([
    { id: id++, text: 'Learn HTML', done: true },
    { id: id++, text: 'Learn JavaScript', done: true },
    { id: id++, text: 'Learn Vue', done: false },
  ])
  function addTodo() {
    if (newTodo.value == '') return
    todos.value.push({ id: id++, text:newTodo.value, done: false})
    newTodo.value = ''
  }
  function removeTodo(todo: any) {
    todos.value = todos.value.filter((t:any) => t!== todo)
  }
  const hideCompleted = ref(false)
  const filteredTodos = computed(() => {
    return hideCompleted.value ? todos.value.filter((t) => !t.done) :
      todos.value
  })

  const pElementRef: any = ref(null)
  onMounted(() =>{
    pElementRef.value.textContent = 'mounted!'
  })

  const todoId = ref(1)
  const todoData = ref(null)
  async function fetchData() {
    todoData.value = null
    const res = await fetch(
       `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
    )
    todoData.value = await res.json()
  }
  fetchData()
  watch(todoId, fetchData)

  const greeting = ref('Hello from parent')

  const childMsg = ref('No child msg yet')

  const msg = ref('from parent')
  
</script>

<style>
.title {
  color: red;
}

.container {
  display: flex;
  border-bottom: 1px white solid;
  margin-bottom: 10px;
  padding-bottom: 10px;
}

.topic {
  width: 250px;
}
</style>

