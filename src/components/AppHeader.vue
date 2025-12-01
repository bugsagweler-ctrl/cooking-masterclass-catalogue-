<template>
  <header class="app-header">
    <h1>Cooking Masterclass</h1>

    <div class="wishlist">
      Wishlist: {{ wishlist.count }}  
      <br>
      Total: R{{ wishlist.totalPrice }}
      <br>
      <span v-if="lastAdded" class="added-msg">
        Added: R{{ lastAdded }}
      </span>
    </div>
  </header>
</template>

<script setup>
import { ref } from 'vue'
import { useWishlistStore } from '@/stores/wishlist'

const wishlist = useWishlistStore()
const lastAdded = ref(null)

// Watch when items change
wishlist.$subscribe((mutation, state) => {
  const item = mutation.events?.key === 'items' ? mutation.events.newValue : null
  
  // When added to wishlist
  if (mutation.type === 'patch object' && mutation.storeId === 'wishlist') {
    if (state.items.length > 0) {
      const lastItem = state.items[state.items.length - 1]
      lastAdded.value = lastItem.price


      setTimeout(() => (lastAdded.value = null), 2000)
    }
  }
})
</script>

<style scoped>
.app-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  background: #f5f5f5;
  padding: 1rem 2rem;
  border-radius: 10px;
}

.wishlist {
  text-align: right;
  flex-shrink: 0; /* Prevent wishlist from shrinking too much */
  min-width: 150px;
}

.added-msg {
  color: green;
  font-weight: bold;
  display: block;
  margin-top: 5px;
}

/* Responsive header */
@media (max-width: 768px) {
  .app-header {
    flex-direction: column;
    text-align: center;
  }
  
  .wishlist {
    text-align: center;
  }
}
</style>
