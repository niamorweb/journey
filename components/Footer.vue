<template>
  <footer class="py-20 px-4 lg:px-10 dark:bg-[#030420]">
    <div class="flex gap-3 flex-col md:flex-row items-center justify-center">
      <span>Build with Nuxt</span>
      <span class="hidden md:flex">-</span>
      <span>niamorweb Copyright © {{ currentYear }}</span>
    </div>
  </footer>
</template>

<script setup>
const currentYear = new Date().getFullYear();
</script>
