<template>
  <v-layout align-center justify-center row fill-height>
    <div class="text-xl-center font-weight-black display-4">
      271
      <span class="display-3">DNI</span>
    </div>
  </v-layout>
</template>

<script>
export default {
  validate({ params }) {
    // Must be a number
    return /^\d+$/.test(params.id)
  }
}
</script>

<style>
</style>
