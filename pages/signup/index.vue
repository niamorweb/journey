<script setup lang="ts">
import { z } from "zod";
import type { FormSubmitEvent } from "#ui/types";

const supabase = useSupabaseClient();

const schema = z.object({
  email: z.string().email("Invalid email"),
  password: z.string().min(8, "Must be at least 8 characters"),
  confirm_password: z.string().min(8, "Must be at least 8 characters"),
  username: z.string().min(3, "Username must be at least 3 characters"),
});

type Schema = z.output<typeof schema>;

const state = reactive({
  email: undefined,
  password: undefined,
  confirm_password: undefined,
  username: undefined,
});

const router = useRouter();

async function onSubmit(event: FormSubmitEvent<Schema>) {
  const { data, error } = await supabase.auth.signUp({
    email: event.data.email,
    password: event.data.password,
  });

  if (error) {
    console.log(error);
    return;
  }

  const user = data.user;

  if (user) {
    const { data: profileData, error: profileError } = await supabase
      .from("profiles")
      // @ts-ignore
      .insert([{ user_id: user.id, username: event.data.username }]);

    console.log("profileData ::: ", profileData);

    if (profileError) {
      console.log(profileError);
      return;
    }

    await router.push("/dashboard/blog");
  }
}
</script>

<template>
  <section class="py-20 px-4 lg:px-10">
    <div
      class="bg-neutral-50 dark:bg-neutral-800 p-8 rounded-2xl flex flex-col gap-8 max-w-[600px] mx-auto"
    >
      <h2 class="text-2xl lg:text-4xl font-bold font-title">
        Create an account
      </h2>
      <UForm
        :schema="schema"
        :state="state"
        class="space-y-4 flex flex-col"
        @submit="onSubmit"
      >
        <UFormGroup label="Email" name="email">
          <UInput v-model="state.email" />
        </UFormGroup>

        <UFormGroup label="Username" name="username">
          <UInput v-model="state.username" />
        </UFormGroup>

        <UFormGroup label="Password" name="password">
          <UInput v-model="state.password" type="password" />
        </UFormGroup>

        <UFormGroup label="Confirm Password" name="confirm_password">
          <UInput v-model="state.confirm_password" type="password" />
        </UFormGroup>

        <UButton class="self-end" size="xl" type="submit">Submit</UButton>

        <div class="mt-4">
          Already have an account ?
          <NuxtLink to="/login" class="text-green-500"
            >Login to your account</NuxtLink
          >
        </div>
      </UForm>
    </div>
  </section>
</template>
