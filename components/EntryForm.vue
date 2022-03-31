<template>
  <div class="mt-8 max-w-3xl shadow p-4 bg-white rounded-lg">
    <form method="post" @submit="submit">
      <div>
        <label for="title" class="block text-sm font-medium text-gray-700">
          Title
        </label>
        <div class="mt-1">
          <input
            id="title"
            v-model="entry.title"
            type="text"
            name="title"
            required
            class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md"
            placeholder="Give it a meaningful title"
          />
        </div>
      </div>
      <div class="mt-4">
        <div class="flex justify-end">
          <label for="text" class="sr-only text-sm font-medium text-gray-700">
            Entry
          </label>
          <span class="text-sm text-gray-500">{{ wordCount }} words</span>
        </div>
        <div class="mt-1">
          <textarea
            id="text"
            v-model="entry.content"
            rows="20"
            required
            name="text"
            placeholder="What's on your mind?"
            class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md"
          />
        </div>
      </div>
      <div>
        <button
          type="submit"
          class="w-full mt-2 flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
        >
          Save
        </button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: 'EntryForm',
  props: {
    currentEntry: {
      type: Object,
      default: () => {
        return { title: '', content: '' }
      },
    },
    action: {
      required: true,
      validator(value) {
        return ['create', 'update'].includes(value)
      },
    },
  },
  data() {
    return {
      entry: this.currentEntry,
      errors: null,
    }
  },
  computed: {
    wordCount() {
      const count = this.entry.content.match(/(\w+)/g)
      return count ? count.length : 0
    },
  },
  methods: {
    async submit(e) {
      e.preventDefault()
      const payload = {
        title: this.entry.title,
        content: this.entry.content,
        is_shared: this.entry.is_shared,
      }
      try {
        let result
        if (this.action === 'create') {
          result = await this.$axios.$post('/api/journal-entries', payload)
        } else {
          result = await this.$axios.$put(
            `/api/journal-entries/${this.entry.id}`,
            payload
          )
        }
        this.$router.push(`/entries/${result.id}`)
      } catch (error) {
        this.errors = error.response.data
      }
    },
  },
}
</script>
