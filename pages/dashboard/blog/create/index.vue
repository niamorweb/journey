<template>
  <div class="flex flex-col gap-10">
    <h1 class="text-2xl lg:text-4xl font-bold font-title">Create a new post</h1>
    <form @submit.prevent="handleSubmit">
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

      <div class="mb-4">
        <label for="content" class="block font-bold mb-2">Content</label>
        <UTextarea
          autoresize
          id="content"
          v-model="content"
          placeholder="Enter content"
        ></UTextarea>
      </div>

      <div class="mb-4">
        <label for="image" class="block font-bold mb-2">Upload Image</label>
        <UInput
          size="xl"
          type="file"
          id="image"
          ref="fileInput"
          @change="handleFileChange"
        />
      </div>

      <UButton size="xl" class="mt-4" type="submit"> Submit </UButton>
    </form>
  </div>
</template>

<script setup>
definePageMeta({
  layout: "dashboard",
});

const user = useSupabaseUser();
const supabase = useSupabaseClient();

import { ref } from "vue";
import { v4 as uuidv4 } from "uuid";

const router = useRouter();

const title = ref("");
const content = ref("");
const selectedFile = ref(null);

const handleFileChange = (event) => {
  const file = event[0];
  selectedFile.value = file;
};

const handleSubmit = async () => {
  if (title.value && content.value && selectedFile && user) {
    const fileExtension = selectedFile.value?.name.split(".").pop();
    console.log("selectedFile :::: ", fileExtension);
    console.log("fileExtension :::: ", fileExtension);

    const filePath = `${user.value.id}/${uuidv4()}.${fileExtension}`;

    const { data: uploadData, error: uploadError } = await supabase.storage
      .from("posts_images")
      .upload(filePath, selectedFile.value);

    if (uploadError) {
      console.log(uploadError);
      return;
    }

    console.log("File uploaded:", uploadData);

    const { data: postData, error: profileError } = await supabase
      .from("posts")
      .insert([
        {
          user_id: user.value.id,
          title: title.value,
          content: content.value,
          image: uploadData.fullPath,
        },
      ]);

    console.log("postData ::: ", postData);

    if (profileError) {
      console.log(profileError);
      return;
    }

    await router.push("/dashboard/blog");
  }
};
</script>
