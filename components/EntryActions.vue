<template>
  <div>
    <div class="flex flex-row gap-2 text-gray-500">
      <div class="hover:text-gray-800">
        <nuxt-link :to="`/entries/${entry.id}/edit`">
          <v-icon small>mdi-pencil</v-icon>
          <span class="text-xs">Edit</span>
        </nuxt-link>
      </div>
      <div class="hover:text-gray-800" @click="toggleDialog">
        <v-icon small>mdi-delete</v-icon>
        <span class="text-xs">Delete</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    entry: {
      type: Object,
      required: true,
    },
  },
  methods: {
    toggleDialog() {
      this.$emit('toggle-dialog')
    },
  },
}
</script>

<style></style>
