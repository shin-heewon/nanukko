<script setup>
import { useApi } from '@/composables/useApi';

const api = useApi();
const { $showToast } = useNuxtApp();
const props = defineProps({
  product: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(["remove-success"]);

const removeWishlist = async () => {
  try {
    await api.post(
      `/my-store/wishlist/remove`,
      props.product,
      {
        params: {
          productId: props.product.productId,
        },
      }
    );
    emit("remove-success");
  } catch (error) {
    console.error("찜목록에서 삭제 실패:", error);
    $showToast("삭제에 실패했습니다. 다시 시도해주세요.");
  }
};

const goToPayment = async () => {
  await navigateTo({
    path: "/payments",
    query: {
      productId: props.product.productId,
    },
  });
};
</script>

<template>
  <div class="wishlist-card">
    <div class="card-content">
      <div class="image-and-info">
        <img :src="product.thumbnamilImage" :alt="product.productName" class="product-image" />
        <div class="product-info">
          <h3 class="product-name">{{ product.productName }}</h3>
          <button class="wish-btn" @click="removeWishlist">
            ♥ <span>{{ product.wishCount }}</span>
          </button>
          <p class="price">{{ product.price.toLocaleString() }}원</p>
        </div>
      </div>
      <div class="button-group">
        <button class="chat-btn">채팅</button>
        <button @click="goToPayment" class="payment-btn">즉시 결제</button>
      </div>
    </div>
  </div>
</template>
