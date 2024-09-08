<script setup>
import { ref, onMounted } from "vue";

const supabase = useSupabaseClient();
const router = useRouter();
const user = useSupabaseUser();
const route = useRoute();
const usernameRoute = route.params.username;
const postIdRoute = route.params.postId;

const profileData = ref([]);
const postData = ref([]);

const generateFormattedDate = (isoDateString) => {
  const date = new Date(isoDateString);
  const formattedDate = date.toLocaleDateString();
  return formattedDate;
};

onMounted(async () => {
  const { data: profileDataResponse, error: profileError } = await supabase
    .from("profiles")
    .select("*")
    .eq("username", usernameRoute);

  if (profileError) {
    console.error("Profile fetch error:", profileError);
    return;
  }

  profileData.value = profileDataResponse || [];

  const { data: postDataResponse, error: postError } = await supabase
    .from("posts")
    .select("*")
    .eq("id", postIdRoute);

  if (postError) {
    console.error("Post fetch error:", postError);
    return;
  }

  postData.value = postDataResponse || [];
});
</script>

<template>
  <section class="py-20 px-10">
    <div
      class="flex flex-col mx-auto items-center justify-center gap-20"
      v-if="profileData.length > 0 && postData.length > 0"
    >
      <div class="flex flex-col gap-8">
        <div class="max-w-[800px] mx-auto w-full">
          <h3 class="text-2xl lg:text-4xl font-bold font-title">
            {{ postData[0].title }}
          </h3>
          <p>Created at: {{ generateFormattedDate(postData[0].created_at) }}</p>
        </div>
        <NuxtImg
          class="max-w-[1000px] w-full aspect-[2/1] rounded-2xl object-cover"
          :src="`https://rpnefuqdghxpqyfhiqcm.supabase.co/storage/v1/object/public/${postData[0].image}`"
        />
        <div class="flex flex-col gap-3 mx-auto max-w-[800px]">
          <p class="text-lg">
            {{ postData[0].content }}
          </p>
        </div>
      </div>

      <NuxtLink
        :to="`/user/${profileData[0].username}`"
        class="flex flex-col gap-4 self-start mx-auto w-full max-w-[800px]"
      >
        <NuxtImg
          class="w-[50px] h-[50px] object-cover rounded-full"
          :src="
            profileData[0].avatar ||
            'https://images.pexels.com/photos/1481581/pexels-photo-1481581.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1'
          "
        />
        <h2 class="text-4xl font-bold">{{ profileData[0].username }}</h2>
        <p class="leading-6">{{ profileData[0].bio }}</p>
      </NuxtLink>
    </div>
    <div v-else>
      <p>User or post unknown</p>
    </div>
  </section>
</template>
