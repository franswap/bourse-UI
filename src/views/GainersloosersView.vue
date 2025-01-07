<script setup lang="ts">
import GainerLoosers from '../components/GainerLoosers.vue';
</script>

<template>
  <div class="gainersloosers">
    <GainerLoosers />
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .gainersloosers {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
