<template>
  <div
    class="product"
    v-on:mouseenter="isHover = true"
    v-on:mouseleave="isHover = false"
  >

    <button
      @click="deleteProduct"
      :class="isHover ? 'hovered' : '' "
      class="delete_product"
    ></button>

    <div class="product_img">
      <img
        v-if="product.image == ''"
        src="../assets/product.png"
        alt="product"
      >

      <img
        v-else
        :src="product.image"
        alt="product"
      >

    </div>

    <div class="product_body">
      <h3 class="product_name">{{product.name}}</h3>

      <p class="product_description">{{product.description}}</p>

      <h4 class="product_price">{{product.price}} руб.</h4>

    </div>
  </div>
</template>
<script>
export default {
  props: {
    product: {
      type: Object,
      required: true,
    },
  },

  data() {
    return {
      isHover: false,
    };
  },

  methods: {
    deleteProduct() {
      this.$emit("deleteProduct", this.product.id);
    },
  },
};
</script>
<style scoped lang="scss">
.product {
  margin-top: 10px;
  position: relative;
  z-index: 1;
  width: 332px;
  min-height: 423px;
  margin: 8px;
  background: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
  align-self: flex-start;
}

.delete_product {
  top: -10px;
  right: -10px;
  position: absolute;
  width: 35px;
  height: 35px;
  border: none;
  cursor: pointer;
  z-index: 2;
  background: none;
  background-image: url("../assets/delete.png");
  background-position: center center;
  visibility: hidden;
  opacity: 0;
  transition: 0.3s;
}

.hovered {
  opacity: 1;
  visibility: visible;
}

.product_img {
  border-radius: 4px 4px 0px 0px;
  height: 200px;
  img {
    width: 100%;
    height: auto;
    max-height: 200px;
  }
}

.product_body {
  padding: 16px 16px 24px 16px;
  display: flex;

  flex-direction: column;
  justify-content: space-between;
}

.product_name {
  font-size: 20px;
  font-weight: 600;
}

.product_description {
  margin-top: 16px;
  font-size: 16px;
  line-height: 20px;
  min-height: 80px;
  font-weight: 400;
}

.product_price {
  margin-top: 32px;
  font-size: 24px;
  font-weight: 600;
}
</style>