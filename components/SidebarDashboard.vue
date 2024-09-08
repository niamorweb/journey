<template>
  <section
    class="bg-neutral-50 dark:bg-neutral-800 sticky top-0 p-4 lg:p-10 flex-1 h-screen flex flex-col justify-between"
  >
    <div class="flex flex-col gap-10">
      <NuxtLink to="/">
        <h2 class="text-xs md:text-base lg:text-3xl font-bold font-title">
          Journey
        </h2>
      </NuxtLink>

      <UVerticalNavigation :links="links">
        <template #default="{ link }">
          <span
            class="group-hover:text-primary relative whitespace-nowrap py-2 px-5 hidden lg:flex"
            >{{ link.label }}</span
          >
          <LucideBookPlus class="flex lg:hidden !text-black" />
        </template>
      </UVerticalNavigation>
    </div>

    <div class="flex flex-col gap-3">
      <UButton
        v-if="profileData"
        class="whitespace-nowrap"
        :to="`/user/${profileData.username}`"
        target="_blank"
      >
        <span class="hidden lg:flex">See my profile</span>
        <LucideUser class="flex lg:hidden" />
      </UButton>
      <UButton @click="handleSignOut" color="white" variant="solid">
        <span class="hidden lg:flex">Log out</span>
        <LucideLogOut class="flex lg:hidden"
      /></UButton>
    </div>
  </section>
</template>

<script setup>
const supabase = useSupabaseClient();
const user = useSupabaseUser();
const router = useRouter();

const { data: profileData, error: profileError } = await supabase
  .from("profiles")
  .select("*")
  .eq("user_id", user.value.id)
  .single();

const links = [
  // {
  //   label: "Dashboard",
  //   to: "/dashboard",
  // },
  {
    label: "Your posts",
    to: "/dashboard/blog",
  },
  {
    label: "Profile",
    to: "/dashboard/profile",
  },
  // {
  //   label: "Settings",
  //   to: "/dashboard/settings",
  // },
];

const handleSignOut = async () => {
  await supabase.auth.signOut();
  router.push("/");
};
</script>
