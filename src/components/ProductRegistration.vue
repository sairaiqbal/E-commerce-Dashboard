<template>
  <div class="bg-white p-8 rounded-2xl shadow-xl max-w-5xl mx-auto mt-12">
    <h2 class="text-3xl font-bold text-purple-700 mb-8 text-center">
      Register New Product
    </h2>

    <form
      @submit.prevent="submit"
      class="grid grid-cols-1 md:grid-cols-2 gap-6"
    >
      <!-- Product Name -->
      <div>
        <label class="form-label">Product Name</label>
        <input
          v-model="product.name"
          type="text"
          placeholder="e.g. Wireless Mouse"
          class="form-input"
          required
        />
      </div>

      <!-- Price -->
      <div>
        <label class="form-label">Price ($)</label>
        <input
          v-model.number="product.price"
          type="number"
          min="0"
          class="form-input"
          required
        />
      </div>

      <!-- Stock -->
      <div>
        <label class="form-label">Initial Stock</label>
        <input
          v-model.number="product.stock"
          type="number"
          min="0"
          class="form-input"
          required
        />
      </div>

      <!-- Platform -->
      <div>
        <label class="form-label">Platform</label>
        <select v-model="product.platform" class="form-input" required>
          <option disabled value="">Select Platform</option>
          <option value="Amazon">Amazon</option>
          <option value="Walmart">Walmart</option>
        </select>
      </div>

      <!-- Description -->
      <div class="md:col-span-2">
        <label class="form-label">Description</label>
        <textarea
          v-model="product.description"
          rows="4"
          class="form-input"
          placeholder="Brief description of the product"
          required
        ></textarea>
      </div>

      <!-- Image Upload -->
      <div>
        <label class="form-label">Upload Image</label>
        <input
          type="file"
          @change="handleImageUpload"
          accept="image/*"
          class="form-input cursor-pointer"
        />
      </div>

      <!-- Image Preview -->
      <div v-if="imagePreview" class="flex items-center justify-center">
        <img
          :src="imagePreview"
          alt="Preview"
          class="w-32 h-32 object-cover rounded-lg border border-purple-300"
        />
      </div>

      <!-- Submit Button -->
      <div class="md:col-span-2 text-center mt-6">
        <button
          type="submit"
          class="bg-purple-600 hover:bg-purple-700 text-white px-8 py-3 rounded-xl font-semibold transition-all"
          :disabled="loading"
        >
          {{ loading ? "Adding..." : "Add Product" }}
        </button>

        <!-- Success Message -->
        <transition name="fade">
          <p v-if="successMessage" class="text-green-600 mt-4 text-sm">
            {{ successMessage }}
          </p>
        </transition>
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const product = ref({
  name: "",
  price: 0,
  stock: 0,
  platform: "",
  description: "",
  image: null,
});

const imagePreview = ref("");
const successMessage = ref("");
const loading = ref(false);

function handleImageUpload(e) {
  const file = e.target.files[0];
  if (file) {
    product.value.image = file;
    imagePreview.value = URL.createObjectURL(file);
  }
}

async function submit() {
  if (!product.value.name || !product.value.platform) {
    alert("Please fill all required fields.");
    return;
  }

  loading.value = true;

  try {
    const payload = {
      title: product.value.name,
      price: product.value.price,
      stock: product.value.stock,
      platform: product.value.platform,
      description: product.value.description,
      // image not supported by dummyjson API
    };

    const response = await axios.post(
      "https://dummyjson.com/products/add",
      payload
    );

    successMessage.value = `✅ "${response.data.title}" added successfully with ID: ${response.data.id}`;

    // Reset form
    product.value = {
      name: "",
      price: 0,
      stock: 0,
      platform: "",
      description: "",
      image: null,
    };
    imagePreview.value = "";

    setTimeout(() => (successMessage.value = ""), 3000);
  } catch (error) {
    console.error("Failed to add product:", error);
    alert("❌ Failed to add product. Please try again.");
  } finally {
    loading.value = false;
  }
}
</script>

<style scoped>
.form-input {
  @apply w-full border border-purple-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-purple-400;
}

.form-label {
  @apply block text-sm font-medium text-gray-700 mb-1;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
