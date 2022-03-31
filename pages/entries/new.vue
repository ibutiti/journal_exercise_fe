<template>
  <div>
    <div class="mt-6 mb-12 md:flex md:items-center md:justify-between">
      <div class="flex-1 min-w-0">
        <h2
          class="text-2xl font-bold leading-7 text-gray-900 sm:text-3xl sm:truncate"
        >
          Add an entry
        </h2>
      </div>
    </div>
    <entry-form action="create" />
  </div>
</template>

<script>
import EntryForm from '~/components/EntryForm.vue'
export default {
  name: 'CreateEntry',
  components: {
    EntryForm,
  },
}
</script>
