<script setup>
const supabase = useSupabaseClient();
const router = useRouter();
const user = useSupabaseUser();

const route = useRoute();
const usernameRoute = route.params.username;

console.log("usernameRoute ::: ", usernameRoute);

const { data: profileData, error } = await supabase
  .from("profiles")
  .select("*")
  .eq("username", usernameRoute);

console.log("profileData ::: ", profileData);
console.log("error ::: ", error);

const { data: postsData, error: postsError } = await supabase
  .from("posts")
  .select("*")
  .eq("user_id", profileData[0].user_id);

const generateFormattedDate = (isoDateString) => {
  const date = new Date(isoDateString);
  const formattedDate = date.toLocaleDateString();
  return formattedDate;
};
</script>

<template>
  <section class="py-20 px-4 lg:px-10">
    <div
      class="flex flex-col items-center justify-center gap-20"
      v-if="profileData.length > 0"
    >
      <div class="flex flex-col gap-4 items-center justify-center">
        <NuxtImg
          class="w-[150px] h-[150px] object-cover rounded-full"
          src="https://images.pexels.com/photos/1481581/pexels-photo-1481581.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1"
        />
        <h2 class="text-2xl lg:text-4xl font-bold">
          {{ profileData[0].username }}
        </h2>
        <p>{{ profileData[0].bio }}</p>
      </div>
      <div
        class="grid sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6 justify-center"
        v-if="postsData"
      >
        <NuxtLink
          :to="`/user/${usernameRoute}/post/${post.id}`"
          class="bg-neutral-50 dark:bg-neutral-800 rounded-2xl overflow-hidden shadow-sm flex flex-col"
          v-for="post in postsData"
        >
          <NuxtImg
            :src="`https://rpnefuqdghxpqyfhiqcm.supabase.co/storage/v1/object/public/${post.image}`"
          />
          <div class="p-6 flex flex-col gap-3">
            <h3 class="text-2xl font-bold">{{ post.title }}</h3>
            <div class="text-container">
              <p>
                {{ post.content }}
              </p>
            </div>

            <p>Created at : {{ generateFormattedDate(post.created_at) }}</p>
          </div>
        </NuxtLink>
      </div>
    </div>
    <div v-else>
      <p>User unknown</p>
    </div>
  </section>
</template>

<style scoped>
.text-container {
  display: -webkit-box;
  -webkit-line-clamp: 2; /* Limite le texte à 2 lignes */
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis; /* Ajoute "..." à la fin si le texte est coupé */
  height: 3.2em; /* Ajustez cette hauteur pour contrôler la hauteur du conteneur */
}
</style>
