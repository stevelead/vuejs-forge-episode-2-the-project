<script setup>
import { debouncedWatch, refDebounced } from "@vueuse/core";
const { fetchProducts } = useProductStore();
const router = useRouter();
const loading = ref(false);
const loadingDebounced = refDebounced(loading, 500);
const productStore = useProductStore();
const filters = computed(() => productStore.filters);
debouncedWatch(
  filters,
  async () => {
    loading.value = true;
    router.push({ query: filters.value });
    await fetchProducts();
    loading.value = false;
  },
  { deep: true, debounce: 200 }
);
</script>
<template>
  <div class="flex items-center gap-2 filters-wrapper">
    <AppSpinner style="transform: translateY(15px)" v-if="loadingDebounced" />
    <div class="form-control">
      <label class="label" for="search">
        <span class="label-text">Search</span>
      </label>
      <input
        id="search"
        v-model="filters.query"
        type="text"
        class="input input-bordered"
      />
    </div>
    <div class="w-full max-w-xs form-control">
      <label class="label" for="filterHeat">
        <span class="label-text">Filter By Heat</span>
      </label>
      <select
        id="filterHeat"
        class="select select-bordered"
        v-model="filters[`fields.heatLevel`]"
      >
        <option value="">All</option>
        <option value="Mild">Mild</option>
        <option value="Medium">Medium</option>
        <option value="Hot">Hot</option>
      </select>
    </div>
    <div class="w-full max-w-xs form-control">
      <label class="label" for="orderBy">
        <span class="label-text">Order by</span>
      </label>
      <select
        class="select select-bordered"
        v-model="filters.order"
        id="orderBy"
      >
        <option value="">None</option>
        <option value="-fields.heatLevel">Heat (Mild First)</option>
        <option value="fields.heatLevel">Heat (Hot First)</option>
        <option value="fields.price">Price (Low to High)</option>
        <option value="-fields.price">Price (High to Low)</option>
      </select>
    </div>
  </div>
</template>

<style scoped>
@media (max-width: 560px) {
  .filters-wrapper {
    display: block;
    width: 100%;
  }
  .form-control {
    width: 100% !important;
    max-width: 100%;
  }
  input,
  select {
    width: 100%;
  }
}
</style>
