<script setup>
const features = [
  {
    title: "User Registration",
    description: "Easily sign up and start creating your blog within minutes.",
  },
  {
    title: "Personalized Blog Page",
    description:
      "Every user gets a custom page showcasing all their blog posts.",
  },
  {
    title: "Easy Content Management",
    description: "Manage your blog posts, edit, and publish them in one place.",
  },
  {
    title: "Responsive Design",
    description: "Your blog looks great on all devices, mobile or desktop.",
  },
  {
    title: "Analytics Dashboard",
    description:
      "Track your blog’s performance and visitor metrics in real-time.",
  },
  {
    title: "SEO Optimization",
    description:
      "Your blog is optimized for search engines to increase visibility.",
  },
];

const pros = [
  {
    title: "Automatic Scaling",
    description:
      "Traffic is automatically routed and load balanced across multiple servers. Your blog scales effortlessly as your audience grows.",
    icon: "scaleIcon", // Remplace par l'icône appropriée
  },
  {
    title: "Exceptionally Affordable",
    description:
      "Enjoy affordable blogging services. Get up to 100,000 visits per day for free, and pay only for higher traffic volumes.",
    icon: "affordableIcon", // Remplace par l'icône appropriée
  },
  {
    title: "Performant Global Network",
    description:
      "Your blog is deployed across a global network, ensuring it runs with low latency for readers anywhere in the world.",
    icon: "networkIcon", // Remplace par l'icône appropriée
  },
  {
    title: "Data Protection",
    description:
      "We use advanced encryption and a multi-factor authentication system to protect your data and blog content.",
    icon: "securityIcon", // Remplace par l'icône appropriée
  },
];

const stepsCreateBlog = [
  {
    title: "Create an account",
    description: "Enter your email, your password and an username.",
  },
  {
    title: "Add new posts",
    description:
      "Create posts about everything you want ( a recipe, a news..).",
  },
  {
    title: "Edit your profile informations",
    description: "Change your biography, your profile picture.",
  },
  {
    title: "Discover your new page created",
    description:
      "Now you can share to everyone your page with your username for they can read your blog.",
  },
];

const user = useSupabaseUser();
</script>

<template>
  <section
    class="relative py-32 px-4 lg:px-16 flex flex-col items-center justify-center overflow-hidden border-t-green-500 border-t-[3px]"
  >
    <div
      class="h-[120px] absolute top-0 left-0 right-0 bg-gradient-to-b from-green-500/20 from-10% to-100% w-full"
    ></div>
    <!-- <div
      class="absolute top-1/4 left-2/3 bg-green-500 blur-[200px] aspect-square w-[200px] lg:w-[400px] rounded-full"
    ></div>
    <div
      class="absolute top-[10%] right-3/4 bg-green-500 blur-[200px] aspect-square w-[100px] lg:w-[200px] rounded-full"
    ></div> -->
    <!-- <div
      class="grid grid-pattern grid-cols-5 absolute left-1/2 -translate-x-1/2 top-1/2 -translate-y-1/2 h-full w-full"
    >
      <div
        v-for="n in 45"
        :key="n"
        class="border border-black/5 h-full w-full"
      ></div>
    </div> -->
    <div class="relative flex items-center justify-center">
      <div
        class="w-full h-full flex flex-col items-center justify-center gap-4"
      >
        <UBadge :ui="{ rounded: 'rounded-full' }">Beta version</UBadge>

        <h1
          v-motion="{
            initial: {
              y: 100,
              opacity: 0,
            },
            enter: {
              y: 0,
              opacity: 1,
            },
          }"
          class="text-3xl lg:text-6xl font-black text-center font-title"
        >
          Start your blogging journey <br />
          Share <span class="text-green-500"> your stories </span>with the world
        </h1>
        <p
          v-motion="{
            initial: {
              y: 100,
              opacity: 0,
            },
            enter: {
              y: 0,
              opacity: 1,
            },
          }"
        >
          Create your personalized blog in just a few steps. Connect with your
          audience and share your unique stories. <br />
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Soluta
          placeat eum obcaecati voluptates. Fugiat libero cumque, nihil quis
          pariatur delectus.
        </p>
        <div
          class="flex items-center gap-3 mt-4 mb-32"
          v-motion="{
            initial: {
              y: 100,
              opacity: 0,
            },
            enter: {
              y: 0,
              opacity: 1,
            },
          }"
        >
          <UButton to="/dashboard" v-if="user">Go to dashboard</UButton>
          <UButton to="/signup" v-else>Get Started Now</UButton>
          <UButton variant="ghost" color="white" to="#presentation"
            >Learn more
          </UButton>
        </div>

        <!-- <NuxtImg
          v-motion="{
            initial: {
              y: 100,
              opacity: 0,
            },
            enter: {
              y: 0,
              opacity: 1,
            },
          }"
          class="rounded-3xl w-full max-w-[1000px] mt-12"
          src="https://cdn.dribbble.com/userupload/13534460/file/original-9c1163d9de14b413750c8bb257152c56.png?resize=1504x1128"
        /> -->
      </div>
    </div>
  </section>

  <section class="px-4 lg:px-16">
    <div class="grid grid-cols-2 lg:grid-cols-4 gap-2">
      <div
        class="flex flex-col gap-1 px-4 relative"
        v-for="(pro, index) in pros"
      >
        <div
          class="w-[1px] h-full absolute left-0 top-0 bottom-0 bg-gradient-to-b from-green-500"
        ></div>
        <div
          class="border-green-500 border bg-gradient-to-b from-green-500/20 h-8 w-8 flex items-center justify-center rounded-lg"
        >
          <div v-if="index === 0">
            <LucideUserPlus class="text-green-500 w-4 h-4" />
          </div>
          <div v-if="index === 1">
            <LucideLayout class="text-green-500 w-4 h-4" />
          </div>
          <div v-if="index === 2">
            <LucideEdit class="text-green-500 w-4 h-4" />
          </div>
          <div v-if="index === 3">
            <LucideSmartphone class="text-green-500 w-4 h-4" />
          </div>
        </div>
        <h3 class="font-bold font-title mt-1">{{ pro.title }}</h3>
        <p class="text-black/50 dark:text-white/70">
          {{ pro.description }}
        </p>
      </div>
    </div>
  </section>

  <section class="py-40 px-4 lg:px-16">
    <div
      v-motion="{
        initial: { y: 100, opacity: 0 },
        enter: { y: 0, opacity: 1 },
        leave: { y: -100, opacity: 0 },
      }"
      class="flex flex-col gap-14 justify-center items-center"
    >
      <div
        class="flex flex-col gap-4 items-center justify-center max-w-[600px] text-center"
      >
        <UBadge :ui="{ rounded: 'rounded-full' }">Get started</UBadge>
        <h2 class="text-2xl lg:text-4xl font-extrabold font-title">
          Create your blog in <span class="text-green-500">few minutes</span>
        </h2>
        <p>
          Our platform offers a simple and intuitive interface for bloggers of
          all levels. Get your voice heard!
        </p>
      </div>
      <div class="grid sm:grid-cols-2 lg:grid-cols-4 gap-10">
        <div
          v-for="step in stepsCreateBlog"
          :key="index"
          class="rounded-xl overflow-hidden flex flex-col gap-5"
        >
          <NuxtImg
            src="https://hub.nuxt.com/_ipx/s_460x259/images/landing/steps/link-cloudflare-account.svg"
          />
          <div class="flex flex-col gap-1">
            <h3 class="font-bold font-title">{{ step.title }}</h3>
            <p class="text-sm">
              {{ step.description }}
            </p>
          </div>
        </div>
      </div>
      <UButton color="black" to="/signup" v-if="user">Go to dashboard</UButton>
      <UButton color="black" to="/dashboard" v-else>Join today</UButton>
    </div>
  </section>

  <section
    id="presentation"
    class="relative border-t-[3px] border-t-green-500 py-40 px-4 lg:px-16 overflow-hidden"
  >
    <div
      class="h-[120px] absolute top-0 left-0 right-0 bg-gradient-to-b from-green-500/20 from-10% to-100% w-full"
    ></div>
    <div class="flex flex-col gap-14 justify-center items-center">
      <div
        class="flex flex-col gap-4 items-center justify-center max-w-[600px] text-center"
      >
        <UBadge :ui="{ rounded: 'rounded-full' }">All-in-one tool</UBadge>
        <h2
          v-motion="{
            initial: {
              y: 100,
              opacity: 0,
            },
            enter: {
              y: 0,
              opacity: 1,
            },
          }"
          class="text-2xl lg:text-4xl font-extrabold font-title"
        >
          Create Your Custom Blog
        </h2>
        <p
          v-motion="{
            initial: {
              y: 100,
              opacity: 0,
            },
            enter: {
              y: 0,
              opacity: 1,
            },
          }"
        >
          Sign up, create a blog, and manage all your posts in one place. Your
          personal page will showcase all your blogs.
        </p>
      </div>
      <div
        v-motion="{
          initial: {
            y: 100,
            opacity: 0,
          },
          enter: {
            y: 0,
            opacity: 1,
          },
        }"
        class="flex items-center"
      >
        <NuxtImg
          class="rounded-3xl max-w-[600px]"
          src="https://cdn.dribbble.com/userupload/13534460/file/original-9c1163d9de14b413750c8bb257152c56.png?resize=1504x1128"
        />
        <NuxtImg
          class="rounded-3xl max-w-[600px] z-10 -translate-x-24 translate-y-24"
          src="https://cdn.dribbble.com/userupload/14256955/file/original-d6d2c7fed27ff19fdc3e2abab8f1a773.png?resize=1504x1128"
        />
      </div>
    </div>
  </section>

  <section class="py-40 px-4 lg:px-16">
    <div
      v-motion="{
        initial: { y: 100, opacity: 0 },
        enter: { y: 0, opacity: 1 },
        leave: { y: -100, opacity: 0 },
      }"
      class="flex flex-col gap-14 justify-center items-center"
    >
      <div
        class="flex flex-col gap-4 items-center justify-center max-w-[600px] text-center"
      >
        <UBadge :ui="{ rounded: 'rounded-full' }">All features</UBadge>
        <h2 class="text-2xl lg:text-4xl font-extrabold font-title">
          Why Choose Us
        </h2>
        <p>
          Our platform offers a simple and intuitive interface for bloggers of
          all levels. Get your voice heard!
        </p>
      </div>
      <div class="grid sm:grid-cols-2 lg:grid-cols-3 gap-10">
        <div
          v-for="(feature, index) in features"
          :key="index"
          class="bg-neutral-50 dark:bg-[#0a0b2a] rounded-xl overflow-hidden p-12 flex flex-col gap-3"
        >
          <div v-if="index === 0">
            <LucideUserPlus class="text-4xl" />
          </div>
          <div v-if="index === 1">
            <LucideLayout class="text-4xl" />
          </div>
          <div v-if="index === 2">
            <LucideEdit class="text-4xl" />
          </div>
          <div v-if="index === 3">
            <LucideSmartphone class="text-4xl" />
          </div>
          <div v-if="index === 4">
            <LucideBarChart2 class="text-4xl" />
          </div>
          <div v-if="index === 5">
            <LucideSearch class="text-4xl" />
          </div>
          <h3 class="text-2xl font-bold font-title">{{ feature.title }}</h3>
          <p>
            {{ feature.description }}
          </p>
        </div>
      </div>
      <UButton color="black" to="/dashboard" v-if="user"
        >Go to dashboard</UButton
      >
      <UButton color="black" to="/signup" v-else>Join Today</UButton>
    </div>
  </section>

  <section class="py-40 px-4 lg:px-16">
    <div class="flex flex-col gap-14 justify-center items-center">
      <div
        class="flex flex-col gap-4 items-center justify-center max-w-[600px] text-center"
      >
        <h2 class="text-2xl lg:text-4xl font-extrabold font-title">
          Free and Open Source.
        </h2>
        <p>
          Our platform is completely free to use and open source. Customize it
          the way you want!
        </p>
        <UButton size="xl" color="black" type="submit"
          ><LucideGithub /> View Source Code</UButton
        >
      </div>
      <div></div>
    </div>
  </section>
</template>

<style scoped>
.grid-pattern {
  mask-image: radial-gradient(circle, black 30%, transparent 70%);
  mask-size: cover;
  mask-repeat: no-repeat;
}
</style>
