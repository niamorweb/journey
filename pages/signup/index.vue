<script setup lang="ts">
import { z } from "zod";
import type { FormSubmitEvent } from "#ui/types";
import { useNuxtApp } from "#app";

// Utilisation du client Supabase
const supabase = useSupabaseClient();

const schema = z.object({
  email: z.string().email("Invalid email"),
  password: z.string().min(8, "Must be at least 8 characters"),
  confirm_password: z.string().min(8, "Must be at least 8 characters"),
  username: z.string().min(3, "Username must be at least 3 characters"), // Nouveau champ username
});

type Schema = z.output<typeof schema>;

// Réactiver l'état des champs
const state = reactive({
  email: undefined,
  password: undefined,
  confirm_password: undefined,
  username: undefined, // Nouveau champ pour le username
});

const router = useRouter();

// Fonction de soumission du formulaire
async function onSubmit(event: FormSubmitEvent<Schema>) {
  // Log des données soumises
  console.log(event.data);

  // Inscription de l'utilisateur avec email et mot de passe
  const { data, error } = await supabase.auth.signUp({
    email: event.data.email,
    password: event.data.password,
  });

  // Si une erreur se produit pendant l'inscription, la loguer
  if (error) {
    console.log(error);
    return;
  }

  // Log des données renvoyées par Supabase
  console.log("data ::: ", data);

  // Si l'inscription est réussie, insérer le username dans la table profiles
  const user = data.user;

  if (user) {
    const { data: profileData, error: profileError } = await supabase
      .from("profiles") // Table "profiles"
      // @ts-ignore
      .insert([
        { user_id: user.id, username: event.data.username }, // Enregistre le username lié à l'utilisateur
      ]);

    // Log du profil inséré
    console.log("profileData ::: ", profileData);

    // Gestion des erreurs lors de l'insertion dans profiles
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
        <!-- Champ pour l'email -->
        <UFormGroup label="Email" name="email">
          <UInput v-model="state.email" />
        </UFormGroup>

        <!-- Champ pour le username -->
        <UFormGroup label="Username" name="username">
          <UInput v-model="state.username" />
        </UFormGroup>

        <!-- Champ pour le mot de passe -->
        <UFormGroup label="Password" name="password">
          <UInput v-model="state.password" type="password" />
        </UFormGroup>

        <!-- Champ pour la confirmation du mot de passe -->
        <UFormGroup label="Confirm Password" name="confirm_password">
          <UInput v-model="state.confirm_password" type="password" />
        </UFormGroup>

        <!-- Bouton de soumission -->
        <UButton class="self-end" size="xl" type="submit">Submit</UButton>
      </UForm>
    </div>
  </section>
</template>
