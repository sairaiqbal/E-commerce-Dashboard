<template>
  <div class="space-y-6">
    <h2 class="text-2xl font-bold text-purple-700">Revenue Analysis</h2>

    <!-- Top Metrics -->
    <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
      <div class="bg-white p-4 rounded shadow hover:bg-purple-200 text-center">
        <div class="text-gray-500">Total Orders</div>
        <div class="text-2xl font-bold text-purple-700">{{ totalOrders }}</div>
      </div>
      <div class="bg-white p-4 rounded shadow hover:bg-purple-200 text-center">
        <div class="text-gray-500">Total Revenue</div>
        <div class="text-2xl font-bold text-purple-700">
          ${{ totalRevenue.toLocaleString() }}
        </div>
      </div>
      <div class="bg-white p-4 rounded shadow hover:bg-purple-200 text-center">
        <div class="text-gray-500">Avg. Order</div>
        <div class="text-2xl font-bold text-purple-700">
          ${{ avgOrderValue.toFixed(2) }}
        </div>
      </div>
    </div>

    <!-- Filter Bar -->
    <div class="flex items-center justify-between flex-wrap gap-4">
      <h3 class="text-lg font-semibold text-gray-700">Revenue Trends</h3>
      <select
        v-model="selectedCategory"
        class="bg-white border border-gray-300 rounded px-4 appearance-none py-2 text-sm focus:outline-none focus:ring-2 focus:ring-purple-500"
      >
        <option value="">All Categories</option>
        <option
          v-for="category in categories"
          :key="category"
          :value="category"
        >
          {{ category }}
        </option>
      </select>
    </div>

    <!-- Charts Grid -->
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
      <div class="bg-white p-4 rounded shadow">
        <h4 class="text-md font-semibold mb-2 text-gray-600">
          Sales Over Time
        </h4>
        <line-chart :data="salesOverTime" class="w-full h-64" />
      </div>
      <div class="bg-white p-4 rounded shadow">
        <h4 class="text-md font-semibold mb-2 text-gray-600">
          Orders Over Time
        </h4>
        <bar-chart :data="ordersOverTime" class="w-full h-64" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";
import axios from "axios";

import LineChart from "./charts/LineChart.vue";
import BarChart from "./charts/BarChart.vue";

const products = ref([]);
const categories = ref([]);
const selectedCategory = ref("");

const totalOrders = ref(0);
const totalRevenue = ref(0);
const avgOrderValue = ref(0);

const salesOverTime = ref([]);
const ordersOverTime = ref([]);

function generateMockTimeSeries(dataLength = 12, maxValue = 10000) {
  const result = [];
  for (let i = 0; i < dataLength; i++) {
    const date = new Date();
    date.setMonth(date.getMonth() - (dataLength - 1 - i)); // Monthly
    result.push({
      date: date.toISOString().slice(0, 10),
      value: Math.floor(Math.random() * maxValue),
    });
  }
  return result;
}

async function fetchProducts() {
  try {
    const response = await axios.get(
      "https://dummyjson.com/products?limit=100"
    );
    products.value = response.data.products;

    // Extract unique categories
    categories.value = Array.from(
      new Set(products.value.map((p) => p.category))
    );

    updateMetrics();
  } catch (error) {
    console.error("Failed to fetch products:", error);
  }
}

function updateMetrics() {
  const filtered = selectedCategory.value
    ? products.value.filter((p) => p.category === selectedCategory.value)
    : products.value;

  totalOrders.value = Math.round(
    filtered.reduce((sum, p) => sum + p.stock * (Math.random() * 10), 0)
  );
  totalRevenue.value = filtered.reduce((sum, p) => {
    const salesCount = Math.floor(p.stock * (Math.random() * 10));
    return sum + salesCount * p.price;
  }, 0);
  avgOrderValue.value =
    totalOrders.value === 0 ? 0 : totalRevenue.value / totalOrders.value;

  salesOverTime.value = generateMockTimeSeries(12, 10000);
  ordersOverTime.value = generateMockTimeSeries(12, 500);
}

onMounted(() => {
  fetchProducts();
});

watch(selectedCategory, () => {
  updateMetrics();
});
</script>
<template>
  <div class="space-y-6">
    <h2 class="text-2xl font-bold text-purple-700">Revenue Analysis</h2>

    <!-- Top Metrics -->
    <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
      <div class="bg-white p-4 rounded shadow text-center">
        <div class="text-gray-500">Total Orders</div>
        <div class="text-2xl font-bold text-purple-700">{{ totalOrders }}</div>
      </div>
      <div class="bg-white p-4 rounded shadow text-center">
        <div class="text-gray-500">Total Revenue</div>
        <div class="text-2xl font-bold text-purple-700">
          ${{ totalRevenue.toLocaleString() }}
        </div>
      </div>
      <div class="bg-white p-4 rounded shadow text-center">
        <div class="text-gray-500">Avg. Order</div>
        <div class="text-2xl font-bold text-purple-700">
          ${{ avgOrderValue.toFixed(2) }}
        </div>
      </div>
    </div>

    <!-- Filter Bar -->
    <div class="flex items-center justify-between flex-wrap gap-4">
      <h3 class="text-lg font-semibold text-gray-700">Revenue Trends</h3>
      <select
        v-model="selectedCategory"
        class="bg-white border border-gray-300 rounded px-4 appearance-none py-2 text-sm focus:outline-none focus:ring-2 focus:ring-purple-500"
      >
        <option value="">All Categories</option>
        <option
          v-for="category in categories"
          :key="category"
          :value="category"
        >
          {{ category }}
        </option>
      </select>
    </div>

    <!-- Charts Grid -->
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
      <div class="bg-white p-4 rounded shadow">
        <h4 class="text-md font-semibold mb-2 text-gray-600">
          Sales Over Time
        </h4>
        <line-chart :data="salesOverTime" class="w-full h-64" />
      </div>
      <div class="bg-white p-4 rounded shadow">
        <h4 class="text-md font-semibold mb-2 text-gray-600">
          Orders Over Time
        </h4>
        <bar-chart :data="ordersOverTime" class="w-full h-64" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";
import axios from "axios";

import LineChart from "./charts/LineChart.vue";
import BarChart from "./charts/BarChart.vue";

const products = ref([]);
const categories = ref([]);
const selectedCategory = ref("");

const totalOrders = ref(0);
const totalRevenue = ref(0);
const avgOrderValue = ref(0);

const salesOverTime = ref([]);
const ordersOverTime = ref([]);

function generateMockTimeSeries(dataLength = 12, maxValue = 10000) {
  const result = [];
  for (let i = 0; i < dataLength; i++) {
    const date = new Date();
    date.setMonth(date.getMonth() - (dataLength - 1 - i)); // Monthly
    result.push({
      date: date.toISOString().slice(0, 10),
      value: Math.floor(Math.random() * maxValue),
    });
  }
  return result;
}

async function fetchProducts() {
  try {
    const response = await axios.get(
      "https://dummyjson.com/products?limit=100"
    );
    products.value = response.data.products;

    // Extract unique categories
    categories.value = Array.from(
      new Set(products.value.map((p) => p.category))
    );

    updateMetrics();
  } catch (error) {
    console.error("Failed to fetch products:", error);
  }
}

function updateMetrics() {
  const filtered = selectedCategory.value
    ? products.value.filter((p) => p.category === selectedCategory.value)
    : products.value;

  totalOrders.value = Math.round(
    filtered.reduce((sum, p) => sum + p.stock * (Math.random() * 10), 0)
  );
  totalRevenue.value = filtered.reduce((sum, p) => {
    const salesCount = Math.floor(p.stock * (Math.random() * 10));
    return sum + salesCount * p.price;
  }, 0);
  avgOrderValue.value =
    totalOrders.value === 0 ? 0 : totalRevenue.value / totalOrders.value;

  salesOverTime.value = generateMockTimeSeries(12, 10000);
  ordersOverTime.value = generateMockTimeSeries(12, 500);
}

onMounted(() => {
  fetchProducts();
});

watch(selectedCategory, () => {
  updateMetrics();
});
</script>
