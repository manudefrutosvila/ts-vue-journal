<script lang="ts" setup>
import DateDisplay from './DateDisplay.vue'
import UseEmojis from '@/composables/UseEmojis'
import type Entry from '@/types/Entry'
import { userInjectionKey } from '@/injectionKeys'
import { inject } from 'vue'

defineProps<{
  entry: Entry
}>()

const user = inject(userInjectionKey)
const { findEmoji } = UseEmojis()
</script>

<template>
  <div class="entry-card">
    <div class="entry-card-body">
      <component width="75" :is="findEmoji(entry.emoji)"></component>
      <div class="entry-text">{{ entry.body }}</div>
    </div>
    <div class="entry-footer">
      <DateDisplay :date="entry.createdAt" class="mr-2" />
      |
      <span class="ml-2">{{ user?.username || 'anonymous' }}</span>
    </div>
  </div>
</template>
