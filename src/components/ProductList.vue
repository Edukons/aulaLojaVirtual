<script setup>
import { onMounted, watch } from 'vue';
import { useProductStore } from '@/stores/product';

import HeartOutline from 'vue-material-design-icons/HeartOutline.vue';
import Star from 'vue-material-design-icons/Star.vue';
import { formatDescription, formatPrice, formatTitle } from '@/helpers/format';

const props = defineProps(['category_id']);
const productStore = useProductStore();

async function getProducts() {
  if (props.category_id) {
    await productStore.getProductsByCategory(props.category_id);
  } else {
    await productStore.getProducts();
  }
}

watch(
  () => props.category_id,
  async () => {
    await getProducts();
  },
);

onMounted(async () => {
  await getProducts();
});
</script>

<template>
  <div class="product-list">
    <router-link :to="{ name: 'ProductAdd' }">
      <button class="icon ">
        <i class="mdi mdi-plus" />
      </button>
    </router-link>
    <div v-if="productStore.products.length === 0">
      <p>Produtos não encontrados!!!</p>
    </div>
    <div
      v-for="product in productStore.products"
      :key="product.id"
      class="product-card"
    >
      <div class="product-img-wrapper">
        <img :src="product.image?.url" alt="product.name" />
        <heart-outline />
      </div>
      <div class="product-title-price">
        <p></p>
        <p></p>
      </div>
      <div class="product-description-stars">
        <p></p>
        <div class="stars">
          <star size="20" />
          <star size="20" />
          <star size="20" />
          <star size="20" />
          <star size="20" />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.icon {
  background-color: #0a2668;
  color: white;
  border: none;
  border-radius: 50%;
  width: 50px;
  height: 50px;

  position: fixed;
  bottom: 12rem;
  right: 20px;
}

.icon:hover {
  background-color: #bac9e8;
  color: #0a2668;
}

.icon i {
  font-size: 2rem;
}

.product-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1.5rem;
  padding: 1rem;
}

.product-card {
  width: 225px;
  font-family: 'Belleza', sans-serif;
}

.product-img-wrapper {
  display: flex;
  justify-content: center;
  align-items: top;
  gap: 0.5rem;
  padding-top: 20px;
  margin-bottom: 1rem;
  background-color: #eeeeee;
  height: 201px;
}

.product-img-wrapper img {
  width: 153px;
  height: 170px;
  object-fit: cover;
}

.product-title-price {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.product-title-price p {
  font-weight: bold;
  font-size: 16px;
  color: #010101;
}

.product-description-stars {
  display: flex;
  flex-direction: column;
  margin-bottom: 1rem;
}

.product-description-stars p {
  font-size: 12px;
  color: #535050;
}
</style>