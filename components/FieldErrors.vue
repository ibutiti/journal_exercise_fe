<template>
  <ul>
    <li
      v-for="(error, index) in fieldErrors"
      :key="index"
      class="text-xs text-red-500"
    >
      {{ error }}
    </li>
  </ul>
</template>

<script>
export default {
  name: 'FieldErrors',
  props: ['fieldErrors'],
}
</script>
