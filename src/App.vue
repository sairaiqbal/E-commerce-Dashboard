<template>
  <div class="min-h-screen flex flex-col md:flex-row bg-gray-100 font-sans">
    <header
      class="flex items-center justify-between bg-purple-700 text-white p-4 md:hidden"
    >
      <div class="text-xl font-bold flex items-center gap-2">
        <svg
          class="w-7 h-7"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M3 3h2l.4 2H21l-4 8H7L5.4 5H3zM7 13l-1.5 7h11m0 0a1 1 0 11-2 0 1 1 0 002 0zm-10 0a1 1 0 11-2 0 1 1 0 002 0z"
          />
        </svg>
        Dashboard
      </div>
      <button
        @click="isSidebarOpen = !isSidebarOpen"
        aria-label="Toggle menu"
        class="focus:outline-none"
      >
        <svg
          v-if="!isSidebarOpen"
          class="w-8 h-8"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M4 6h16M4 12h16M4 18h16"
          />
        </svg>
        <svg
          v-else
          class="w-8 h-8"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M6 18L18 6M6 6l12 12"
          />
        </svg>
      </button>
    </header>

    <aside
      :class="[
        'fixed inset-y-0 left-0 z-50 w-64 transform bg-purple-700 text-white flex flex-col p-4 space-y-4 transition-transform duration-300 ease-in-out',
        isSidebarOpen ? 'translate-x-0' : '-translate-x-full',
        'md:static md:translate-x-0 md:w-64',
      ]"
    >
      <div class="text-2xl font-bold mb-6 hidden md:flex items-center gap-2">
        <svg
          class="w-7 h-7"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M3 3h2l.4 2H21l-4 8H7L5.4 5H3zM7 13l-1.5 7h11m0 0a1 1 0 11-2 0 1 1 0 002 0zm-10 0a1 1 0 11-2 0 1 1 0 002 0z"
          />
        </svg>
        Dashboard
      </div>

      <button
        @click="
          () => {
            page = 'revenue';
            isSidebarOpen = false;
          }
        "
        :class="navClass('revenue')"
      >
        <IconChartBar class="w-5 h-5 mr-2" /> Revenue
      </button>

      <button
        @click="
          () => {
            page = 'inventory';
            isSidebarOpen = false;
          }
        "
        :class="navClass('inventory')"
      >
        <IconBoxes class="w-5 h-5 mr-2" /> Inventory
      </button>

      <button
        @click="
          () => {
            page = 'product';
            isSidebarOpen = false;
          }
        "
        :class="navClass('product')"
      >
        <IconPlusCircle class="w-5 h-5 mr-2" /> Register Product
      </button>
    </aside>

    <div
      v-if="isSidebarOpen"
      class="fixed inset-0 bg-black bg-opacity-30 z-40 md:hidden"
      @click="isSidebarOpen = false"
    />

    <main class="flex-1 overflow-y-auto p-6 bg-white min-h-screen">
      <h1 class="text-2xl font-bold text-purple-700 mb-4">
        E-commerce Dashboard
      </h1>

      <RevenuePage v-if="page === 'revenue'" />
      <InventoryPage v-if="page === 'inventory'" />
      <ProductRegistration v-if="page === 'product'" />
    </main>
  </div>
</template>

<script setup>
import { ref } from "vue";
import RevenuePage from "./components/RevenuePage.vue";
import InventoryPage from "./components/InventoryPage.vue";
import ProductRegistration from "./components/ProductRegistration.vue";

import {
  ChartBarIcon as IconChartBar,
  ArchiveBoxIcon as IconBoxes,
  PlusCircleIcon as IconPlusCircle,
} from "@heroicons/vue/24/outline";

const page = ref("revenue");
const isSidebarOpen = ref(false);

const navClass = (target) => {
  return [
    "flex items-center px-4 py-2 rounded hover:bg-purple-600 transition",
    page.value === target ? "bg-purple-600" : "",
  ];
};
</script>

<style scoped>
body {
  margin: 0;
}
</style>
