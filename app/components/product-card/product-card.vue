<script setup lang="ts">
import type { ProductFragment } from '@@/types/shopify';

// Props
const props = defineProps<{
  product: ProductFragment;
}>();

// Helpers
const helpers = useShopifyHelpers();

// Computed
const colorOption = computed(() => helpers.getColorOption(props.product.options));
const colorOptionName = computed(() => colorOption.value?.optionValues[0]?.name);

// Flatten connections
const mediaItems = computed(() => flattenConnection(props.product.media));
</script>

<template>
  <NuxtLink
    :to="`/products/${product.handle}`"
    class="relative flex flex-col gap-4"
  >
    <ProductCardTags :product="product" />
    <ProductCardMedia :media-items="mediaItems" />
    <div class="flex flex-col gap-2">
      <div>
        <h2 class="uppercase">{{ product.title }}</h2>
        <h3 v-if="colorOption">{{ colorOptionName }}</h3>
      </div>
      <PriceDisplay
        :price="product.priceRange.minVariantPrice"
        :compare-at-price-range="product.compareAtPriceRange.minVariantPrice"
      />
    </div>
  </NuxtLink>
</template>
