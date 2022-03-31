<template>
  <div class="lg:col-span-10 xl:col-span-10">
    <entry-list-item
      v-for="entry in entries"
      :key="entry.id"
      class="my-3"
      :entry="entry"
    />
  </div>
</template>

<script>
import EntryListItem from './EntryListItem.vue'
export default {
  name: 'EntriesList',
  components: {
    EntryListItem,
  },
  props: {
    entries: {
      type: Array,
      required: true,
    },
  },
}
</script>

<style></style>
