<template>
  <header
    class="top-0 sticky bg-neutral-100/80 dark:bg-[#030420e0] backdrop-blur-xl z-30"
  >
    <nav class="flex items-center justify-between py-3 px-4 lg:px-10">
      <h3 class="text-xl font-bold font-title">Journey</h3>
      <div class="flex items-center gap-4">
        <UButton to="/dashboard/blog" v-if="user">Go to my dashboard</UButton>
        <UButton to="/login" v-else>Login</UButton>
      </div>
    </nav>
  </header>
</template>

<script setup>
const user = useSupabaseUser();

const links = [
  {
    label: "Recettes",
    href: "/recipes",
    isMainAction: false,
  },
  {
    label: "Créer mon espace",
    href: "/signup",
    isMainAction: true,
  },
];
</script>
