<script lang="ts" setup>
import EmojiField from '@/components/EmojiField.vue'
import ArrowCircleRight from '@/assets/icons/arrow-circle-right.svg'
import { ref, computed, onMounted, inject } from 'vue'
import type Emoji from '@/types/Emoji'
import type Entry from '@/types/Entry'
import { userInjectionKey } from '@/injectionKeys'

const emit = defineEmits<{
  (e: '@create', entry: Entry): void
}>()

const MAX_CHARS = 280

const body = ref('')
const emoji = ref<Emoji | null>(null)
const textareaRef = ref<HTMLTextAreaElement | null>(null)
const user = inject(userInjectionKey)

const charCount = computed<number>(() => body.value.length)

const handleTextarea = (e: Event) => {
  const textarea = e.target as HTMLTextAreaElement

  if (textarea.value.length <= 280) {
    body.value = textarea.value
    return
  }

  body.value = textarea.value = textarea.value.substring(0, MAX_CHARS)
}

const handleSubmit = () => {
  emit('@create', {
    body: body.value,
    emoji: emoji.value,
    createdAt: new Date(),
    userId: 1,
    id: Math.random(),
  })
  body.value = ''
  emoji.value = null
}

onMounted(() => textareaRef.value?.focus())
</script>

<template>
  <form class="entry-form" @submit.prevent="handleSubmit">
    <textarea
      ref="textareaRef"
      :value="body"
      @keyup="handleTextarea"
      :placeholder="`New Journal Entry for ${user?.username || 'anonymous'}`"
    ></textarea>
    <EmojiField v-model="emoji" />
    <div class="entry-form-footer">
      <span>{{ charCount }} / {{ MAX_CHARS }}</span>
      <button>Remember <ArrowCircleRight width="20" /></button>
    </div>
  </form>
</template>
