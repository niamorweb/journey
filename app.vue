<template>
  <div class="bg-neutral-100 dark:bg-[#030420] min-h-screen">
    <NuxtRouteAnnouncer />
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>

<script setup>
definePageMeta({
  layout: "basic",
});
</script>

<style>
html {
  scroll-behavior: smooth;
  font-family: "Dm Sans", sans-serif;
}

.font-title {
  font-family: "Bricolage Grotesque", sans-serif;
}
</style>
