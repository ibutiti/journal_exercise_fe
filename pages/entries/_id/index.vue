<template>
  <div class="mt-8">
    <div
      data-app
      class="flex flex-col items-center text-lg max-w-prose mx-auto"
    >
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
      <entry-actions :entry="entry" @toggle-dialog="toggleDeleteDialog" />
      <delete-entry-dialog
        :open="deleteDialogOpen"
        :entry="entry"
        @toggle-dialog="toggleDeleteDialog"
      />
    </div>
    <article class="mt-6 prose prose-indigo prose-lg text-gray-500 mx-auto">
      {{ entry.content }}
    </article>
  </div>
</template>

<script>
import { DateTime } from 'luxon'
import EntryActions from '~/components/EntryActions.vue'
import DeleteEntryDialog from '~/components/DeleteEntryDialog.vue'
export default {
  name: 'ViewEntry',
  components: { EntryActions, DeleteEntryDialog },
  async asyncData({ params, $axios, route, redirect }) {
    try {
      const entry = await $axios.$get(`/api/journal-entries/${params.id}`)
      return { entry }
    } catch (error) {
      return { error: error.response.data.message }
    }
  },
  data() {
    return {
      entry: null,
      error: null,
      deleteDialogOpen: false,
    }
  },
  computed: {
    createdAt() {
      const dt = DateTime.fromISO(this.entry.created_at)
      return dt.toLocaleString(DateTime.DATE_MED_WITH_WEEKDAY)
    },
  },
  methods: {
    toggleDeleteDialog() {
      this.deleteDialogOpen = !this.deleteDialogOpen
    },
  },
}
</script>
