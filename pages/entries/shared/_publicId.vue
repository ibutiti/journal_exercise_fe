<template>
  <div>
    <div v-if="error">{{ error.message }}</div>
    <div v-else class="mt-8">
      <div class="flex flex-col items-center text-lg max-w-prose mx-auto">
        <h1>
          <span
            class="mt-2 block text-3xl text-center leading-8 font-extrabold tracking-tight text-gray-900 sm:text-4xl"
          >
            {{ entry.title }}
          </span>
          <span class="block text-sm text-center text-gray-600 tracking-wide">
            {{ createdAt }}
          </span>
        </h1>
      </div>
      <article class="mt-6 prose prose-indigo prose-lg text-gray-500 mx-auto">
        {{ entry.content }}
      </article>
    </div>
  </div>
</template>

<script>
import { DateTime } from 'luxon'
export default {
  name: 'ViewPublicEntry',
  auth: false,
  async asyncData({ params, $axios }) {
    try {
      const entry = await $axios.$get(
        `/api/journal-entries/shared/${params.publicId}`
      )
      return { entry }
    } catch (error) {
      if (error.response.status === 404) {
        return {
          error: { message: 'Journal entry not found' },
        }
      }
      return { error: error.response.data.message }
    }
  },
  data() {
    return {
      entry: null,
      error: null,
    }
  },
  computed: {
    createdAt() {
      const dt = DateTime.fromISO(this.entry.created_at)
      return dt.toLocaleString(DateTime.DATE_MED_WITH_WEEKDAY)
    },
  },
}
</script>
