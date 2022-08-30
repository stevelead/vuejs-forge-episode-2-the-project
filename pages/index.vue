<script setup>
const productStore = useProductStore();
useAsyncData("products", async () => productStore.fetchProducts());
</script>
<template>
  <div>
    <HomeHero />

    <div class="flex justify-end px-10 mt-10">
      <ProductFilters />
    </div>

    <div
      v-if="productStore.products"
      class="flex-wrap items-stretch grid-cols-2 p-10 gap-7 sm:grid md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 justify-items-stretch"
    >
    <TransitionGroup name="products">
      <ProductCard
        v-for="product in productStore.products"
        :product="product"
        :key="product.sys.id"
        class="mb-5"
      />
    </TransitionGroup>
    </div>
  </div>
</template>

<style>
  .product-card {
    transition: all 0.5s ease-in-out;
  }
  .products-enter-from {
    transform: scale(0.5) translatey(-80px);
    opacity: 0;
  }
  .products-leave-to {
    transform: translatey(30px);
    opacity: 0;
  }
  .products-leave-active {
    position: absolute;
    z-index: -1;
  }
  </style>