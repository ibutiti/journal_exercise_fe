<template>
  <div>
    <div class="flex flex-row gap-2 text-gray-500">
      <div class="hover:text-gray-800">
        <nuxt-link :to="`/entries/${entry.id}/edit`">
          <v-icon small>mdi-pencil</v-icon>
          <span class="text-xs">Edit</span>
        </nuxt-link>
      </div>
      <div class="hover:text-gray-800">
        <div class="text-center">
          <v-menu offset-y :close-on-content-click="false">
            <template v-slot:activator="{ on, attrs }">
              <div v-bind="attrs" v-on="on">
                <v-icon small>mdi-share-variant</v-icon>
                <span class="text-xs">Share</span>
              </div>
            </template>
            <v-list>
              <v-list-item>
                <v-switch
                  @click="toggleSharing"
                  :loading="switchIsLoading"
                  v-model="switchState"
                  label="Enable sharing"
                ></v-switch>
              </v-list-item>
              <v-list-item @click="copyShareLink" v-if="currentEntry.is_shared">
                <v-icon small>mdi-share-variant</v-icon>
                <span>Copy share link</span>
              </v-list-item>
            </v-list>
          </v-menu>
        </div>
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
  data() {
    return {
      currentEntry: this.entry,
      switchIsLoading: false,
      switchState: this.entry.is_shared,
    }
  },
  methods: {
    toggleDialog() {
      this.$emit('toggle-dialog')
    },
    async toggleSharing() {
      this.switchIsLoading = true
      try {
        const payload = {
          title: this.currentEntry.title,
          content: this.currentEntry.content,
          is_shared: !this.currentEntry.is_shared,
        }
        console.log(payload)
        const result = await this.$axios.$put(
          `/api/journal-entries/${this.entry.id}`,
          payload
        )
        this.currentEntry = result
      } catch (error) {}
      this.switchIsLoading = false
    },
    copyShareLink() {
      console.log(window.location.host)
      const shareLink = `http://${window.location.host}/entries/shared/${this.currentEntry.public_id}`
      navigator.clipboard.writeText(shareLink)
    },
  },
}
</script>

<style></style>
