<template>
  <div>

    <transition-group
      name="products_container"
      tag="div"
      class="products_container"
    >

      <ProductItem
        @deleteProduct="deleteProduct"
        v-for="item in poductsList"
        :key="item.id"
        :product="item"
      />
    </transition-group>

  </div>
</template>
<script>
import ProductItem from "./ProductItem.vue";

export default {
  components: { ProductItem },
  props: {
    poductsList: {
      type: Array,
      required: true,
    },
  },

  methods: {
    deleteProduct(id) {
      this.$emit("deleteProduct", id);
    },
  },
};
</script>
<style scoped lang="scss">
.select_block {
  margin-bottom: 15px;
}

.products_container {
  flex-wrap: wrap;
  display: flex;
  transition: 0.3s;
  transform: rotate(360deg);
  margin-top: -8px;
  margin-left: 8px;
}

.products_container-move {
  transition: 0.8s ease;
}
.products_container-leave-active {
  transition: 0.35s;
  opacity: 0;
}

@keyframes entrance {
  0% {
    transform: scale(0);
    opacity: 0;
  }

  100% {
    opacity: 1;
    transform: scale(1);
  }
}

.products_container-enter-to {
  transition: 0.5s;
  animation: entrance 0.5s;
}

@media screen and (max-width: 730px) {
  .products_container {
    justify-content: center;
  }
}
</style>