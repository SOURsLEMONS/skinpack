<script setup>
import { ref } from 'vue'

const count = ref(0)
</script>

<template>
  <h1>{{ msg }}</h1>
  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <h1 v-if="count <= 5">H1</h1>
    <h1 v-else>h1</h1>
    <h2 v-for="i in H2">{{ i.name }} ----- {{ i.n }}
    <p v-if="i.n==1">我是{{ i }}</p>
    </h2>
    <p>
      我是一个组件
    </p>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
