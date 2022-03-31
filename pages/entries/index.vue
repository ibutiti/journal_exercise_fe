<template>
  <div>
    <div class="mt-6 mb-12 md:flex md:items-center md:justify-between">
      <div class="flex-1 min-w-0">
        <h2
          class="text-2xl font-bold leading-7 text-gray-900 sm:text-3xl sm:truncate"
        >
          Journal Entries
        </h2>
      </div>
      <div class="mt-4 flex md:mt-0 md:ml-4">
        <nuxt-link
          to="/entries/new"
          class="ml-3 inline-flex items-center px-4 py-2 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
        >
          Add entry
        </nuxt-link>
      </div>
    </div>
    <div v-if="!entries.length" class="text-center">
      <svg
        class="mx-auto h-12 w-12 text-gray-400"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        aria-hidden="true"
      >
        <path
          vector-effect="non-scaling-stroke"
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M9 13h6m-3-3v6m-9 1V7a2 2 0 012-2h6l2 2h6a2 2 0 012 2v8a2 2 0 01-2 2H5a2 2 0 01-2-2z"
        />
      </svg>
      <h3 class="mt-2 text-sm font-medium text-gray-900">No journal entries</h3>
      <p class="mt-1 text-sm text-gray-500">
        Get started by creating a new entry.
      </p>
      <div class="mt-6">
        <nuxt-link
          to="/entries/new"
          type="button"
          class="inline-flex items-center px-4 py-2 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
        >
          New entry
        </nuxt-link>
      </div>
    </div>
    <div>
      <entries-list :entries="entries" />
    </div>

    <ul></ul>
  </div>
</template>

<script>
import EntriesList from '~/components/EntriesList.vue'
export default {
  name: 'EntriesPage',
  components: {
    EntriesList,
  },

  async asyncData({ $axios }) {
    const entries = await $axios.$get('/api/journal-entries')
    return { entries }
  },
  data() {
    return {
      entries: [],
    }
  },
}
</script>
