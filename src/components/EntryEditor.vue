<script lang="ts" setup>
import { computed, inject, onMounted, ref } from 'vue'

import EmojiField from '@/components/EmojiField.vue'
import ArrowCircleRight from '@/assets/icons/arrow-circle-right.svg'
import type Emoji from '@/types/Emoji'
import Entry from '@/types/Entry';
import { userInjectionKey } from '@/injectionKeys'

const MAX_CHARACTERS = 280
const body = ref('')
const emoji = ref<Emoji | null>(null)
const textAreaRef = ref<HTMLTextAreaElement | null>(null)
const charCount = computed(() => body.value.length)
const user = inject(userInjectionKey)

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

onMounted(() => textAreaRef.value?.focus())

</script>
<template>
  <form
    class="entry-form"
    @submit.prevent="handleEntrySubmit"
    >
    <textarea
      :value="body"
      ref="textAreaRef"
      @keyup="handleTextInput"
      :placeholder="`New Journal Entry for ${user?.username || 'anonymous'}`"
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
