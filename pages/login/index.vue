<script setup lang="ts">
import { z } from "zod";
import type { FormSubmitEvent } from "#ui/types";

const supabase = useSupabaseClient();

const schema = z.object({
  email: z.string().email("Invalid email"),
  password: z.string().min(8, "Must be at least 8 characters"),
});

type Schema = z.output<typeof schema>;

const state = reactive({
  email: undefined,
  password: undefined,
});

async function onSubmit(event: FormSubmitEvent<Schema>) {
  // Do something with data
  console.log(event.data);
  const { data, error } = await supabase.auth.signInWithPassword({
    email: event.data.email,
    password: event.data.password,
  });
  console.log("data ::: ", data);

  if (error) console.log(error);
}
</script>

<template>
  <section class="py-20 px-4 lg:px-10">
    <div
      class="bg-neutral-50 dark:bg-neutral-800 p-8 rounded-2xl flex flex-col gap-8 max-w-[600px] mx-auto"
    >
      <h2 class="text-2xl lg:text-4xl font-bold font-title">
        Login to your account
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

        <UFormGroup label="Password" name="password">
          <UInput v-model="state.password" type="password" />
        </UFormGroup>

        <UButton class="self-end" size="xl" type="submit"> Submit </UButton>

        <div class="mt-4">
          Don't have an account ?
          <NuxtLink to="/signup" class="text-green-500"
            >Create an account</NuxtLink
          >
        </div>
      </UForm>
    </div>
  </section>
</template>
