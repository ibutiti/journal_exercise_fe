<template>
  <div class="bg-white px-4 py-6 shadow sm:p-6 sm:rounded-lg">
    <div data-app class="flex space-x-3 justify-between">
      <div>
        <h2 class="text-xl font-medium text-gray-900 hover:underline flex-0">
          <nuxt-link :to="'/entries/' + entry.id">{{ entry.title }}</nuxt-link>
        </h2>
        <time class="text-xs text-gray-500" :datetime="entry.created_at">
          {{ createdAt }}
        </time>
      </div>
      <div>
        <entry-actions :entry="entry" @toggle-dialog="toggleDeleteDialog" />
      </div>
    </div>
    <div class="mt-2 text-sm text-gray-700 h-15 text-clip">
      {{ entry.content.slice(0, 400) }}
    </div>
    <delete-entry-dialog
      :open="deleteDialogOpen"
      :entry="entry"
      @toggle-dialog="toggleDeleteDialog"
    />
  </div>
</template>

<script>
import { DateTime } from 'luxon'
import DeleteEntryDialog from './DeleteEntryDialog.vue'
import EntryActions from './EntryActions.vue'

export default {
  name: 'EntryListItem',
  components: { DeleteEntryDialog, EntryActions },
  props: {
    entry: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
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

<style></style>
