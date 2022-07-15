<template>
  <div class="template">

    <div class="header">
      <button
        class="add_product__header adaptive_btn"
        @click="isVisibleModal = true"
      >Добавление товара</button>

      <div class="select_block">

        <select
          class="select"
          v-model="selectedSortType"
        >
          <option value=""></option>
          <option
            selected
            value="time"
          >Времени</option>
          <option value="max">Дороже</option>
          <option value="min">Дешевле</option>
          <option value="name">Название</option>
        </select>
      </div>

    </div>

    <div class="main">

      <div
        class="sidebar"
        @click="isVisibleModal = false"
        :class="{'visible' : isVisibleModal}"
      >
        <ProductForm
          @click.stop=""
          @addNewProduct="addNewProduct"
        />
      </div>

      <ProductsList
        :poductsList="productsList"
        @deleteProduct="deleteProduct"
      />
    </div>

  </div>
</template>

<script>
import ProductForm from "./components/ProductForm.vue";
import ProductsList from "./components/ProductsList.vue";

export default {
  name: "App",
  components: { ProductForm, ProductsList },

  data() {
    return {
      isVisibleModal: false,
      selectedSortType: "time",
      productsList: [
        {
          id: 1,
          name: "Телефон",
          image:
            "https://cdn-dynmedia-1.microsoft.com/is/image/microsoftcorp/Samsung-Galaxy-S20-Family-CC?wid=517&hei=291&fit=crop",
          description:
            "Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строкь",
          price: 14000,
          timeCreated: Date.now(),
        },
        {
          id: 2,
          image:
            "https://www.apple.com/ru/apple-watch-series-3/images/site/meta/gps/og__fbb5ued3v9qy.png",
          name: "Apple Watch",
          description:
            "Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строкь",
          price: 5300,
          timeCreated: Date.now(),
        },
        {
          id: 3,
          image:
            "https://www.ixbt.com/img/n1/news/2022/2/2/MIJIA-Electric-Scooter-3-Lite-3_large.png",
          name: "Самокат",
          description:
            "Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строкь",
          price: 8400,
          timeCreated: Date.now(),
        },
        {
          id: 4,
          image:
            "https://www.iphones.ru/wp-content/uploads/2022/01/K3_0123505.jpg",
          name: "Клавиатура",
          description:
            "Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строкь",
          price: 3323,
          timeCreated: Date.now(),
        },
        {
          id: 5,
          image:
            "https://static.razer.ru/238949/Seiren--V2-Pro-Render-[2021]-v03.png",
          name: "Микрофон",
          description:
            "Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строкь",
          price: 12222,
          timeCreated: Date.now(),
        },
      ],
    };
  },

  mounted() {
    let cashed = JSON.parse(localStorage.getItem("productsList"));

    if (cashed.length > this.productsList.length) {
      this.productsList = cashed;
    }
  },

  methods: {
    deleteProduct(id) {
      this.productsList = this.productsList.filter(
        (product) => product.id != id
      );
    },

    addNewProduct(product) {
      this.productsList.unshift(product);

      this.sort();
    },

    sort() {
      switch (this.selectedSortType) {
        case "time": {
          this.productsList = this.productsList.sort((a, b) =>
            a.timeCreated > b.timeCreated ? -1 : 1
          );
          break;
        }
        case "min": {
          this.productsList = this.productsList.sort((a, b) =>
            a.price > b.price ? 1 : -1
          );
          break;
        }
        case "max": {
          this.productsList = this.productsList.sort((a, b) =>
            a.price > b.price ? -1 : 1
          );
          break;
        }
        case "name": {
          this.productsList = this.productsList.sort((a, b) =>
            a.name > b.name ? 1 : -1
          );
          break;
        }
      }
    },
  },

  watch: {
    productsList: {
      handler() {
        localStorage.setItem("productsList", JSON.stringify(this.productsList));
      },
      deep: true,
    },

    selectedSortType() {
      console.log("sort");
      this.sort();
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@600&family=Source+Sans+Pro:wght@400;600&display=swap");

* {
  padding: 0;
  margin: 0;
  font-family: "Source Sans Pro";
  box-sizing: border-box;
}

#app {
  background: rgb(163, 214, 174);
  min-height: 100vh;
  margin: 0 auto;
  padding-top: 32px;
}

.template {
  max-width: 1440px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  padding: 0 20px 100px 20px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.add_product__header {
  font-size: 28px;
  font-weight: 600;
  background: none;
  border: none;
}

.main {
  margin-top: 16px;
  align-items: flex-start;
  display: flex;
}

.sidebar {
  position: relative;
  min-width: 332px;
}

.select {
  font-size: 12px;
  color: #b4b4b4;
  padding: 10px;
  background: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
}

@media screen and (max-width: 730px) {
  .main {
    align-content: center;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    position: relative;
  }

  .adaptive_btn {
    background: #fffefb;
    border-radius: 10px;
    color: #b4b4b4;
    padding: 10px 20px;
    cursor: pointer;
    transition: 0.3s;
    display: block;
    border: none;
    font-size: 28px;

    &:hover,
    &:focus {
      color: rgb(163 214 174);
    }
  }

  .sidebar {
    top: 0;
    z-index: 10;
    left: 0;
    position: fixed;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 100vh;
    background: rgba($color: #000000, $alpha: 0.6);
    visibility: hidden;
    opacity: 0;
    transition: 0.3s;
  }

  .visible {
    visibility: visible;
    opacity: 1;
  }
}

@media screen and (max-width: 470px) {
  .header {
    flex-direction: column;
  }

  .select_block {
    margin-top: 20px;
  }
}
</style>
