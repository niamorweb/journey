<script setup>
definePageMeta({
  layout: "dashboard",
});

import { ref, onMounted } from "vue";

const supabase = useSupabaseClient();
const router = useRouter();
const user = useSupabaseUser();
const route = useRoute();
const postId = route.params.postId;

const title = ref("");
const content = ref("");
const previousPostImage = ref("");
const selectedFile = ref(null);

const postData = ref(null); // Initialise postData à null
const errorMessage = ref(""); // Pour gérer les messages d'erreur

onMounted(async () => {
  if (!user.value) {
    await router.push("/login");
    return;
  }

  const { data: postDataResponse, error: postError } = await supabase
    .from("posts")
    .select("*")
    .eq("id", postId)
    .single();

  if (postError || !postDataResponse) {
    errorMessage.value = "Post not found or an error occurred.";
    console.error("Post fetch error:", postError);
    return;
  }

  if (user.value.id !== postDataResponse.user_id) {
    await router.push("/dashboard/");
  } else {
    postData.value = postDataResponse;
    title.value = postDataResponse.title;
    content.value = postDataResponse.content;
    previousPostImage.value = postDataResponse.image;
  }
});

import { v4 as uuidv4 } from "uuid";

const handleFileChange = (event) => {
  const file = event[0];
  selectedFile.value = file;
};

const handleSubmit = async () => {
  if (title.value && content.value && user.value) {
    let filePath = previousPostImage.value;

    if (selectedFile.value) {
      const fileExtension = selectedFile.value.name.split(".").pop();
      const newFilePath = `${user.value.id}/${uuidv4()}.${fileExtension}`;
      console.log("previousPostImage ::: ", previousPostImage.value);

      if (previousPostImage.value) {
        const shortPath = previousPostImage.value.replace("posts_images/", "");

        const { error: removeError } = await supabase.storage
          .from("posts_images")
          .remove([shortPath]);

        if (removeError) {
          console.error("Error deleting previous image:", removeError);
        }
      }

      const { data: uploadData, error: uploadError } = await supabase.storage
        .from("posts_images")
        .upload(newFilePath, selectedFile.value);

      if (uploadError) {
        console.error("Upload error:", uploadError);
        return;
      }

      console.log("File uploaded:", uploadData);

      filePath = uploadData.fullPath;
    }

    const { data: updatedPostData, error: updateError } = await supabase
      .from("posts")
      .update({
        title: title.value,
        content: content.value,
        image: filePath,
      })
      .eq("id", postId);

    if (updateError) {
      console.error("Update error:", updateError);
      return;
    }

    console.log("Post updated:", updatedPostData);

    // Redirige vers le tableau de bord après la mise à jour
    await router.push("/dashboard/blog");
  }
};
</script>

<template>
  <section>
    <div v-if="postData" class="flex flex-col gap-10 w-full">
      <h1 class="text-2xl lg:text-4xl font-bold font-title">Edit your post</h1>
      <form class="w-full flex flex-col" @submit.prevent="handleSubmit">
        <!-- Champ Titre -->
        <div class="mb-4">
          <label for="title" class="block font-bold mb-2">Title</label>
          <UInput
            size="xl"
            type="text"
            id="title"
            v-model="title"
            placeholder="Enter title"
          />
        </div>

        <!-- Champ Contenu (Textarea) -->
        <div class="mb-4">
          <label for="content" class="block font-bold mb-2">Content</label>
          <UTextarea
            autoresize
            size="xl"
            id="content"
            v-model="content"
            placeholder="Enter content"
          ></UTextarea>
        </div>

        <div class="mb-4">
          <label for="image" class="block font-bold mb-2">Previous image</label>
          <NuxtImg
            class="w-[200px] object-cover"
            :src="`https://rpnefuqdghxpqyfhiqcm.supabase.co/storage/v1/object/public/${postData.image}`"
          />
        </div>

        <div class="mb-4">
          <label for="image" class="block font-bold mb-2">Change Image</label>
          <UInput
            size="xl"
            type="file"
            id="image"
            ref="fileInput"
            @change="handleFileChange"
          />
        </div>

        <UButton size="xl" class="self-start mt-4" type="submit">
          Submit
        </UButton>
      </form>
    </div>
    <div v-else>
      <p>{{ errorMessage || "Nothing to show" }}</p>
    </div>
  </section>
</template>
