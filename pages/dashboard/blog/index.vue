<script setup>
definePageMeta({
  layout: "dashboard",
});

const toast = useToast();

const supabase = useSupabaseClient();
const router = useRouter();
const user = useSupabaseUser();
const postsData = ref([]);
const modalsState = ref({}); // Stocker l'état d'ouverture de chaque modal par post

const fetchPosts = async () => {
  const { data, error } = await supabase
    .from("posts")
    .select("*")
    .eq("user_id", user.value?.id);

  if (error) {
    console.error(error);
  } else {
    postsData.value = data;
    // Initialiser l'état de chaque modal à "false"
    postsData.value.forEach((post) => {
      modalsState.value[post.id] = false;
    });
  }
};

await fetchPosts();

const generateFormattedDate = (isoDateString) => {
  const date = new Date(isoDateString);
  return date.toLocaleDateString();
};

const deletePost = async (post) => {
  const shortPath = post.image.replace("posts_images/", "");

  const { error: removeError } = await supabase.storage
    .from("posts_images")
    .remove([shortPath]);
  if (removeError) {
    console.error("Error deleting image:", removeError);
  }

  const { error: deleteError } = await supabase
    .from("posts")
    .delete()
    .eq("id", post.id);

  if (deleteError) {
    console.error("Error deleting post:", deleteError);
  } else {
    toast.add({ title: "Post deleted" });
    await fetchPosts();
  }
};
</script>

<template>
  <section class="flex flex-col gap-10">
    <div
      class="flex flex-col gap-3 lg:flex-row items-start lg:items-center lg:justify-between"
    >
      <h2 class="text-2xl lg:text-4xl font-bold font-title">All your posts</h2>
      <UButton to="/dashboard/blog/create">Create a new post</UButton>
    </div>
    <div
      class="grid sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6"
      v-if="postsData"
    >
      <div
        class="bg-neutral-50 dark:bg-neutral-800 rounded-2xl overflow-hidden shadow-sm flex flex-col"
        v-for="post in postsData"
        :key="post.id"
      >
        <NuxtImg
          class="h-[200px] object-cover"
          :src="`https://rpnefuqdghxpqyfhiqcm.supabase.co/storage/v1/object/public/${post.image}`"
        />
        <div class="p-6 flex flex-col gap-3">
          <h3>{{ post.title }}</h3>
          <p>Created at : {{ generateFormattedDate(post.created_at) }}</p>
          <div class="flex items-center gap-3 self-end mt-4">
            <UButton
              color="gray"
              label="Delete"
              @click="modalsState[post.id] = true"
            />
            <UModal v-model="modalsState[post.id]">
              <div class="p-8 flex flex-col gap-2 justify-center items-center">
                <h3 class="text-2xl font-semibold">Are you sure ?</h3>
                <p>This action cannot be undone</p>
                <div class="mt-4 flex items-center gap-3">
                  <UButton @click="modalsState[post.id] = false"
                    >Cancel</UButton
                  >
                  <UButton @click="deletePost(post)">Delete this post</UButton>
                </div>
              </div>
            </UModal>

            <UButton color="gray" :to="`/dashboard/blog/edit/${post.id}`"
              >Edit</UButton
            >
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
