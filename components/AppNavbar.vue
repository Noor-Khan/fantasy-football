<template>
  <header class="bg-black/80 text-white/90">
    <nav
      class="mx-auto flex max-w-7xl items-center justify-between p-6 lg:px-8"
      aria-label="Global"
    >
      <a href="#" class="-m-1.5 p-1.5 flex items-center space-x-3">
        <span class="sr-only">Your Company</span>
        <img class="h-8 w-auto" src="~/assets/img/logo.png" alt="" />
        <h2>Fantasy Football</h2>
      </a>
    </nav>
  </header>
</template>

<script setup>
import { ref } from "vue";

const navigation = [
  { name: "Product", href: "#" },
  { name: "Features", href: "#" },
  { name: "Marketplace", href: "#" },
  { name: "Company", href: "#" },
];
</script>
