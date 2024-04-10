<script setup lang="ts">
import { provide, reactive } from 'vue'
import { userInjectionKey } from './injectionKeys'
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

provide(userInjectionKey, user)

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
        <EntryCard :entry="e" />
      </li>
    </ul>
  </main>
</template>
