<template>
  <div class="text-center">
    <v-dialog persistent :value="open" width="500">
      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          Delete entry?
        </v-card-title>

        <v-card-text>
          Are you sure you want to delete your
          <span class="font-medium">"{{ entry.title }}"</span> entry?
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="deleteEntry"> Delete </v-btn>
          <v-btn color="primary" text @click="toggleDialog"> Cancel </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
export default {
  props: {
    open: {
      type: Boolean,
      required: true,
    },
    entry: {
      type: Object,
      required: true,
    },
  },
  methods: {
    toggleDialog() {
      this.$emit('toggle-dialog')
    },
    async deleteEntry() {
      try {
        const response = await this.$axios.delete(
          `/api/journal-entries/${this.entry.id}`
        )
        if (response.status === 200) {
          if (this.$route.path === '/entries') {
            this.toggleDialog()
            this.$nuxt.refresh()
          }
          this.$router.push('/entries')
        }
      } catch (error) {}
    },
  },
}
</script>
