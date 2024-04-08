<script lang="ts" setup>
import { computed, ref } from 'vue'

import EmojiField from '@/components/EmojiField.vue'
import ArrowCircleRight from '@/assets/icons/arrow-circle-right.svg'
import type Emoji from '@/types/Emoji'
import Entry from '@/types/Entry';

const MAX_CHARACTERS = 280
const body = ref('')
const emoji = ref<Emoji | null>(null)
const charCount = computed(() => body.value.length)

const emit = defineEmits<{
  (e: "create", payload: Entry): void;
}>();

function handleTextInput (e: Event) {
  const textArea = e.target as HTMLTextAreaElement

  if (textArea.value.length <= MAX_CHARACTERS) {
    body.value = textArea.value;
  } else {
    body.value = textArea.value = textArea.value.substring(0, MAX_CHARACTERS)
  }
}

function handleEntrySubmit () {
  emit('create', {
        body: body.value,
        emoji: emoji.value,
        createdAt: new Date(),
        userId: 1,
        id: Math.random()
      })
  body.value = ''
  emoji.value = null
}

</script>
<template>
  <form
    class="entry-form"
    @submit.prevent="handleEntrySubmit"
    >
    <textarea
      :value="body"
      @keyup="handleTextInput"
      placeholder="New Journal Entry for danielkelly_io"
    ></textarea>
    <EmojiField
      v-model="emoji"
    />
    <div class="entry-form-footer">
      <span>{{ charCount }} / {{ MAX_CHARACTERS }}</span>
      <button>Remember <ArrowCircleRight width="20" /></button>
    </div>
  </form>
</template>
