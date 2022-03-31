<template>
  <div class="mt-8">
    Edit entry
    <entry-form :current-entry="entry" action="update" />
  </div>
</template>

<script>
import EntryForm from '~/components/EntryForm.vue'
export default {
  name: 'EditEntry',
  components: {
    EntryForm,
  },
  async asyncData({ params, $axios }) {
    try {
      const entry = await $axios.$get(`/api/journal-entries/${params.id}`)
      return { entry }
    } catch (error) {
      return { error: error.response.data.message }
    }
  },
}
</script>
