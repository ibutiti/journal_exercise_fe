<template>
  <div class="bg-green-900 pt-24 m-20">
    Hello world
    <div v-if="$auth.loggedIn">{{ $auth.user.name }}</div>
  </div>
</template>

<script>
export default {
  name: 'GoalsPage',
}
</script>
