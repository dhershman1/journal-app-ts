<script setup lang="ts">
import { reactive } from 'vue'
import TheHeader from '@/components/TheHeader.vue'
import EntryEditor from './components/EntryEditor.vue'
import EntryCard from '@/components/EntryCard.vue'
import User from './types/User'
import Entry from './types/Entry'

const entries: Entry[] = reactive([])
const user: User = reactive({
  id: 1,
  username: 'dhershman_io',
  settings: []
})

console.log(user)

function handleCreateEntry (entry: Entry) {
  entries.unshift(entry)
}
</script>

<template>
  <main class="container m-auto p-10">
    <TheHeader />
    <EntryEditor @create="handleCreateEntry"/>
    <ul>
      <li v-for="e in entries" :key=e.id>
        <EntryCard />
      </li>
    </ul>
  </main>
</template>
