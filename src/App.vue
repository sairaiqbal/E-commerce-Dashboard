<template>
  <div class="h-screen flex bg-gray-100 font-sans">
    <!-- Sidebar -->
    <nav class="w-64 bg-purple-700 text-white flex flex-col p-4 space-y-4">
      <div class="text-2xl font-bold mb-6 flex items-center gap-2">
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

      <button @click="page = 'revenue'" :class="navClass('revenue')">
        <IconChartBar class="w-5 h-5 mr-2" /> Revenue
      </button>

      <button @click="page = 'inventory'" :class="navClass('inventory')">
        <IconBoxes class="w-5 h-5 mr-2" /> Inventory
      </button>

      <button @click="page = 'product'" :class="navClass('product')">
        <IconPlusCircle class="w-5 h-5 mr-2" /> Register Product
      </button>
    </nav>

    <!-- Main Content -->
    <div class="flex-1 overflow-y-auto p-6 bg-white">
      <h1 class="text-2xl font-bold text-purple-700 mb-4">
        E-commerce Dashboard
      </h1>
      <RevenuePage v-if="page === 'revenue'" />
      <InventoryPage v-if="page === 'inventory'" />
      <ProductRegistration v-if="page === 'product'" />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import RevenuePage from "./components/RevenuePage.vue";
import InventoryPage from "./components/InventoryPage.vue";
import ProductRegistration from "./components/ProductRegistration.vue";

// Heroicons as components
import {
  ChartBarIcon as IconChartBar,
  ArchiveBoxIcon as IconBoxes,
  PlusCircleIcon as IconPlusCircle,
} from "@heroicons/vue/24/outline";

const page = ref("revenue");

// Utility class function for active nav buttons
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
