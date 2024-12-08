<script setup lang="ts">
type TodoItem = {
  userId: number
  id: number
  title: string
  completed: boolean
}

const state = reactive<{
  id: number
  loading: boolean
  data: TodoItem | null
}>(
  {
    id: 1,
    loading: false,
    data: null
  }
)

function increment() {
  state.id++
}

async function fetchTodo() {
  state.loading = true
  try {
    const res = await fetch(`https://jsonplaceholder.typicode.com/todos/${state.id}`)
    state.data = await res.json()
  }
  finally {
    state.loading = false
  }
}

// NG: watch(state.id, fetchTodo, { immediate: true })
// refのときはgetter関数不要。reactiveのときは必要
watch(() => state.id, fetchTodo, { immediate: true })

// watchEffectでも動くが、どの値を監視しているのか明示できない。新旧の値を取得できない
// watchEffect(() => {
//   fetchTodo()
// })
</script>

<template>
  <div>
    <p>ID: {{ state.id }}</p>
    <button type="button" :disabled="state.loading" @click="increment">
      次の TODO アイテムを取得
    </button>
    <p v-if="state.loading">
      Loading...
    </p>
    <pre v-else>{{ state.data }}</pre>
  </div>
</template>
