<template>
  <div>
    <nav-bar />
    <div class="container h-full mx-auto sm:px-6 lg:px-8">
      <Nuxt />
    </div>
  </div>
</template>

<script>
import NavBar from '../components/NavBar.vue'
export default {
  name: 'DefaultLayout',
  components: { NavBar },
}
</script>
