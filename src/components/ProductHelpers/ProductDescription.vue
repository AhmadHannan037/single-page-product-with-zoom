<!-- 
System: Single Product page with zoom
Organization: XYZ
Developer:Ahmad Hanan
Date: 10/29/2023
Purpose: This component display the product details to user
props: product (Object)
-->
<template>
  <div>
    <!-- Product Details -->
    <div class="flex justify-between">
      <!-- Product Brand -->
      <h2 class="text-xs text-gray-500 uppercase lg:text-base">
        {{ product.brand }}
      </h2>
      <!-- Product Status -->
      <div class="text-xs lg:text-base">
        <span class="text-green-700">{{ product.status }}</span>
      </div>
    </div>

    <!-- Product Name and Model -->
    <div class="mt-2">
      <h1 class="text-base font-bold leading-tight lg:text-2xl">
        {{ product.name }}
      </h1>
      <div class="mt-2 text-xs text-gray-500 uppercase">
        {{ product.model }}
      </div>
    </div>

    <!-- Product Price -->
    <div class="flex py-2 mt-2 lg:mt-3">
      <h1 class="text-base font-bold leading-tight lg:text-2xl">
        {{ product.price }}
      </h1>
    </div>

    <!-- Separator -->
    <div class="mt-2 border-t border-gray-300 lg:flex lg:border-t-2"></div>

    <!-- Product Details (Sim, Condition, Color, Network, Storage) -->
    <div class="py-3 text-gray-900">
      <span class="font-semibold capitalize">sim:</span>
      <span class="uppercase">{{ product.sim }}</span>
    </div>

    <!-- Product Condition -->
    <div class="py-3 text-gray-900">
      <span class="font-semibold capitalize">condition:</span>
      <span class="uppercase">{{ product.condition }}</span>
    </div>

    <!-- Product Color -->
    <div class="py-3 text-gray-900">
      <span class="font-semibold capitalize">colour:</span>
      <span class="uppercase">{{ product.colour }}</span>
    </div>

    <!-- Product Network -->
    <div class="py-3 text-gray-900">
      <span class="font-semibold capitalize">network:</span>
      <span class="uppercase">{{ product.network }}</span>
    </div>

    <!-- Product Storage -->
    <div class="py-3 text-gray-900">
      <span class="font-semibold capitalize">storage:</span>
      <span class="uppercase">{{ product.storage }}</span>
    </div>

    <!-- Separator -->
    <div class="pb-3 border-b border-gray-300 lg:flex lg:border-b-2"></div>
  </div>
</template>

<script>
export default {
  props: {
    // Accepts a product object as a prop
    product: Object,
  },
};
</script>