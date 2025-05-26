<template>
  <div>
    <h2 class="text-2xl font-semibold text-purple-700 mb-6">
      Inventory Management
    </h2>

    
    <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
      <input
        v-model="search"
        placeholder="Search products..."
        class="border border-purple-300 px-4 py-2 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-400"
        :disabled="loading"
      />

      <select
        v-model="platformFilter"
        class="border border-purple-300 px-4 py-2 rounded-lg appearance-none bg-white focus:outline-none focus:ring-2 focus:ring-purple-400"
        :disabled="loading"
      >
        <option value="">All Platforms</option>
        <option v-for="p in platforms" :key="p" :value="p">{{ p }}</option>
      </select>

      <select
        v-model="sortBy"
        class="border border-purple-300 px-4 py-2 rounded-lg appearance-none bg-white focus:outline-none focus:ring-2 focus:ring-purple-400"
        :disabled="loading"
      >
        <option value="title">Sort by Name</option>
        <option value="stock">Sort by Stock</option>
      </select>
    </div>

    
    <div v-if="loading" class="text-center py-10 text-purple-600">
      <svg
        class="animate-spin h-8 w-8 mx-auto mb-2 text-purple-600"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
      >
        <circle
          class="opacity-25"
          cx="12"
          cy="12"
          r="10"
          stroke="currentColor"
          stroke-width="4"
        ></circle>
        <path
          class="opacity-75"
          fill="currentColor"
          d="M4 12a8 8 0 018-8v4a4 4 0 00-4 4H4z"
        ></path>
      </svg>
      <p class="text-lg font-medium">Fetching products, please wait...</p>
    </div>

    
    <table
      v-else-if="filteredAndSorted.length > 0"
      class="w-full min-w-[600px] bg-white shadow-md rounded-lg overflow-hidden"
    >
      <thead>
        <tr class="bg-purple-100 text-purple-700">
          <th class="text-left p-3">Product</th>
          <th class="text-left p-3">Platform</th>
          <th class="text-left p-3">Stock</th>
          <th class="text-left p-3">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="item in filteredAndSorted"
          :key="item.id"
          :class="item.stock < 10 ? 'bg-red-50' : 'bg-white'"
        >
          <td class="p-3">{{ item.title }}</td>
          <td class="p-3">{{ item.platform || "Unknown" }}</td>
          <td class="p-3">
            <input
              v-model.number="item.stock"
              type="number"
              class="w-24 border border-purple-300 rounded px-2 py-1 focus:ring-2 focus:ring-purple-400"
            />
          </td>
          <td class="p-3">
            <button
              @click="updateStock(item)"
              class="bg-purple-600 hover:bg-purple-700 text-white px-3 py-1 rounded-lg text-sm"
              :disabled="loading"
            >
              Update
            </button>
            <div v-if="item.stock < 10" class="text-red-600 text-sm mt-1">
              Low stock! Forecast restock soon.
            </div>
          </td>
        </tr>
      </tbody>
    </table>

   
    <div v-else class="text-center text-purple-600 py-8">
      <p class="text-lg font-medium">
        No products match your search or filters.
      </p>
      <p class="text-sm text-gray-500">
        Try changing the search term or filter options.
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import axios from "axios";

const search = ref("");
const platformFilter = ref("");
const sortBy = ref("title");
const inventory = ref([]);
const platforms = ref([]);
const loading = ref(true);

onMounted(async () => {
  loading.value = true;
  try {
    const res = await axios.get("https://dummyjson.com/products?limit=20");
    inventory.value = res.data.products.map((p) => ({
      ...p,
      platform: Math.random() > 0.5 ? "Amazon" : "Walmart",
    }));
    // console.log("inventory "  ,inventory.value)
    platforms.value = [...new Set(inventory.value.map((p) => p.platform))];
    // console.log("saira" , platforms.value)

  } catch (error) {
    console.error("Failed to fetch products:", error);
  } finally {
    loading.value = false;
  }
});

const filteredAndSorted = computed(() => {
  let result = inventory.value.filter((item) =>
    item.title.toLowerCase().includes(search.value.toLowerCase())
  );

  if (platformFilter.value) {
    result = result.filter((item) => item.platform === platformFilter.value);
  }

  result = [...result].sort((a, b) => {
    if (sortBy.value === "title") return a.title.localeCompare(b.title);
    if (sortBy.value === "stock") return a.stock - b.stock;
    return 0;
  });

  return result;
});

async function updateStock(item) {
  try {
    const response = await axios.put(
      `https://dummyjson.com/products/${item.id}`,
      {
        stock: item.stock,
      }
    );

    alert(`✅ Stock for "${item.title}" updated to ${response.data.stock}`);
  } catch (error) {
    console.error("Update failed:", error);
    alert(`❌ Failed to update stock for "${item.title}".`);
  }
}
</script>
