<script setup>
definePageMeta({
  layout: "dashboard",
});

const toast = useToast();

const input = ref(undefined);
const textarea = ref(undefined);

const errorMsg = ref(undefined);

const user = useSupabaseUser();
const supabase = useSupabaseClient();

const validateForm = async () => {
  if (!input.value || input.value.length < 4) {
    return "Username must be at least 3 characters long";
  }
  const { data: profileData, error: profileError } = await supabase
    .from("profiles")
    .select("*")
    .eq("username", input.value);

  if (profileData > 0) {
    return "This username is already taken";
  }
  return null;
};

const { data: profileData, error: profileError } = await supabase
  .from("profiles")
  .select("*")
  .eq("user_id", user.value.id);

input.value = profileData[0].username;
textarea.value = profileData[0].bio;

const form = ref(null);

async function onSubmit(event) {
  event.preventDefault();

  const error = await validateForm();
  errorMsg.value = error;

  if (error) {
    console.log("eee");

    toast.add({ title: error });
    return;
  }

  const { data: profileUpdateData, error: profileUpdateError } = await supabase
    .from("profiles")
    .update({
      username: input.value,
      bio: textarea.value,
    })
    .eq("user_id", user.value.id);
}
</script>

<template>
  <section>
    <div class="flex flex-col gap-10">
      <h1 class="text-2xl lg:text-4xl font-bold font-title">
        Your profile informations
      </h1>
      <UForm
        ref="form"
        :schema="schema"
        :state="state"
        class="space-y-4"
        @submit="onSubmit"
      >
        <UFormGroup name="input" label="Your username">
          <UInput size="xl" v-model="input" />
        </UFormGroup>

        <UFormGroup name="textarea" label="Your bio">
          <UTextarea autoresize size="xl" v-model="textarea" />
        </UFormGroup>

        <p v-if="errorMsg" class="text-red-500">{{ errorMsg }}</p>
        <UButton class="mt-4" size="xl" type="submit"> Submit </UButton>
      </UForm>
    </div>
  </section>
</template>
