<template>
  <div class="product_form_container">

    <form class="product_form">

      <!-- NAME -->
      <label for=""><span class="decor_container">Наименование товара<span class="red_dot"></span></span></label>

      <input
        v-model="newProduct.name"
        @input="validation"
        @blur="validation"
        name="name"
        class="product_form__input"
        :class="{'warning' : !validateStatus.name}"
        placeholder="Введите наименование товара"
        type="text"
      >

      <transition-group name="required_field">
        <p
          class="required_field"
          v-if="!validateStatus.name"
        >Поле является обязательным</p>
      </transition-group>

      <!-- DESCRIPTION -->
      <label for="">Описание товара</label>
      <textarea
        v-model="newProduct.description"
        class="product_form__input product_form__textarea"
        placeholder="Введите описание товара"
        type="text"
      ></textarea>

      <!-- IMAGE -->
      <label for=""><span class="decor_container">Ссылка на изображение товара<span class="red_dot"></span></span></label>

      <input
        v-model="newProduct.image"
        @input="validation"
        @blur="validation"
        name="image"
        class="product_form__input"
        placeholder="Введите ссылку"
        type="text"
      >

      <transition-group name="required_field">
        <p
          class="required_field"
          v-if="!validateStatus.image"
        >Поле является обязательным</p>
      </transition-group>

      <!-- PRICE -->
      <label for=""><span class="decor_container">Цена товара<span class="red_dot"></span></span></label>

      <input
        @input="validation"
        @blur="validation"
        name="price"
        v-model="newProduct.price"
        class="product_form__input product__price_input"
        placeholder="Введите цену"
        type="number"
        min="0"
      >

      <transition-group name="required_field">
        <p
          class="required_field"
          v-if="!validateStatus.price"
        >Поле является обязательным</p>
      </transition-group>

      <button
        :disabled="!validated"
        @click.prevent="addNewProduct"
        class="product_form__add_product"
      >Добавить товар
      </button>

    </form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      validated: false,

      validateStatus: {
        name: true,
        image: true,
        price: true,
      },

      newProduct: {
        id: "",
        name: "",
        image: "",
        description: "",
        price: "",
        timeCreated: "",
      },
    };
  },

  methods: {
    addNewProduct() {
      this.newProduct.id = Date.now();
      this.newProduct.timeCreated = Date.now();
      this.$emit("addNewProduct", this.newProduct);
      this.newProduct = {
        id: "",
        name: "",
        description: "",
        image: "",
        price: "",
        timeCreated: "",
      };

      this.validated = false;
    },

    validation(e) {
      let targetName = e.target.name;

      // PRICE (т.к тут number а не text)
      if ((targetName == "price") & (this.newProduct.price > 1)) {
        this.validateStatus.price = true;
      } else if (
        (targetName == "price") & (this.newProduct.price < 1) ||
        (targetName == "price") & (this.newProduct.price == "")
      ) {
        this.validateStatus.price = false;
      }

      // Проверка тектовых полей
      if (this.newProduct[targetName].length <= 3) {
        this.validateStatus[targetName] = false;
      } else if (this.newProduct[targetName].length > 3) {
        this.validateStatus[targetName] = true;
      }

      this.checkValidate();
    },

    checkValidate() {
      // true + true = 2
      let numRes =
        this.validateStatus.name +
        this.validateStatus.image +
        this.validateStatus.price;

      if (
        (numRes == 3) &
        (this.newProduct.name.length > 3) &
        (this.newProduct.image.length > 3) &
        (this.newProduct.price > 1)
      ) {
        this.validated = true;
      } else this.validated = false;
    },
  },
};
</script>
<style scoped lang="scss">
.product_form_container {
  position: fixed;
  max-width: 332px;
  margin-right: 8px;
}

.product_form_container__header {
  font-size: 28px;
  font-weight: 600;
  margin-bottom: 16px;
}

.product_form {
  border: 1px solid #fff;
  padding: 24px;
  display: flex;
  flex-direction: column;
  text-align: left;
  background: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
  width: 332px;

  label {
    font-size: 10px;
    font-weight: 400;
    margin-bottom: 5px;
  }
}

.decor_container {
  position: relative;
}

.red_dot {
  position: absolute;
  top: 0;
  right: -5px;
  width: 5px;
  height: 5px;
  border-radius: 100%;
  background: #ff8484;
}

.product_form__input {
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  border: none;
  padding: 11px 16px;
  color: #3f3f3f;
  outline: none;
  margin-bottom: 10px;
}

// ПРЕДУПРЕЖДЕНИЯ

@keyframes require_entrance {
  0% {
    opacity: 0;
  }

  100% {
    opacity: 1;
  }
}

.required_field-enter-active {
  animation: require_entrance 1s;
  transition: 0.3s;
}
.required_field-leave-active {
  opacity: 0;
  transition: 0.3s;
}

.required_field {
  transition: 0.3s;
  color: red;
  font-size: 10px;
  margin-bottom: 5px;
}
.warning {
  border: 1px solid red;
  outline: red;
}

.product_form__textarea {
  resize: none;
  height: 108px;
}

// Отключение стрелочек в input[number]
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}

.product_form__add_product {
  background: #7bae73;
  border-radius: 10px;
  border: none;
  padding: 10px 90px;
  font-size: 12px;
  font-weight: 600;
  font-family: "Inter";
  cursor: pointer;
  transition: 0.3s;
  color: #fff;

  &:disabled {
    background: #eeeeee;
    color: #b4b4b4;
  }
}
::placeholder {
  color: #b4b4b4;
}
</style>