<template>
  <div class="flex-row container">
    <div class="form flex-col-md-3">

      <div class="forma">

        <p class="h-add-product">Добавление товара</p>

        <p class="h-name-product required"> Наименование товара </p>
        <input v-model="name" 
        class="nameproduct" placeholder="Введите наименование товара"
        name="name" for="name"/>

        <p class="h-product-description">Введите описание товара</p>
        <textarea v-model.trim="products.text" class="product-description" placeholder="Введите описание товара"/>

        <p class="h-enter-link required">Ссылка на изображение товара</p>
        <input v-model="img" class="enter-link" placeholder="Введите ссылку"
        name="img" for="img"/>

        
        <p class="h-price-product required" required="required">Цена товара</p>
        <input v-model="price" class="price-product" placeholder="Введите цену"
        name="price" for="price" type="number"/>

        <button @click="addProduct" class="add-button"
        :class="{ active: addButtonActive }">Добавить товар</button>

      </div>

      <div class="error">
        <p v-if="errors.length">
        <b>Пожалуйста исправьте указанные ошибки:</b>
        <ul>
          <li v-for="error in errors"
          :key="error.id">{{ error }}</li>
        </ul>
        </p>
      </div>

    </div>

    <div class="products-container flex-col-md-9">

      <div class="filters">
        <select v-model="selectedProducts" class="filter">
          <option>по порядку</option>
          <option>по возрастанию цен</option>
          <option>по убыванию цен</option>
          <option>по наименованию</option>
        </select>
      </div>

      <div class="products-grid">
        
        <div v-for="product in sorting()"
        :key="product.id" class="product-container">
          <div class="product">
            <div class="product-photo">
            <img :src="product.thumbnail" alt="">
            <button class="product-remove" @click="removeProduct(product)"></button>
          </div>
          <b class="h-description"> {{ product.title }} </b>
          <textarea class="description" v-model="product.description" disabled/> 
          <b class="price">{{ product.price }} ₽ </b>
          <b class="rating"> &#9733 {{ product.rating }}  </b>
          </div>
        </div>
        
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: "DefaultLayout",
  props: {
    msg: String,
  },
  data() {
    return {
      products: [],
      newProduct: "",
      errors: [],
      name: null,
      img: null,
      price: null,
      selectedProducts: "по порядку",
    };
  },
  methods: {
    createNewId() {
      let maxId = Math.max(...this.products.map((i) => i.id)); //Определяет максимальный id в массиве
      let newId = maxId > 0 ? maxId + 1 : 1; //Добавляет 1 к максимальному id, если же массив пуст, выводит 1
      return newId;
    },
    addProduct: function () {
      if (this.name && this.img && this.price) {
        this.products.push({
          id: this.createNewId(),
          name: this.name,
          img: this.img,
          text: this.products.text,
          price: this.price,
          basket: false,
        });
      }
      this.errors = [];
      if (!this.name) {
        this.errors.push("Требуется указать имя.");
      }
      if (!this.img) {
        this.errors.push("Требуется указать ссылку.");
      }
      if (!this.price) {
        this.errors.push("Требуется указать цену.");
      }
      this.name = "";
      this.img = "";
      this.products.text = "";
      this.price = "";
    },
    removeProduct: function (product) {
      this.products.splice(this.products.indexOf(product), 1);
    },
    sorting: function () {
      const sortByName = function (d1, d2) {
        return d1.title.toLowerCase() > d2.title.toLowerCase() ? 1 : -1;
      };
      const sortById = function (d1, d2) {
        return d1.id > d2.id ? 1 : -1;
      };
      const sortByPrice = function (d1, d2) {
        return d1.price > d2.price ? 1 : -1;
      };
      const sortByPriceReverse = function (d1, d2) {
        return d1.price < d2.price ? 1 : -1;
      };
      if (this.selectedProducts === "по возрастанию цен") {
        return this.products.slice().sort(sortByPrice);
      }
      if (this.selectedProducts === "по убыванию цен") {
        return this.products.slice().sort(sortByPriceReverse);
      }
      if (this.selectedProducts === "по наименованию") {
        return this.products.slice().sort(sortByName);
      } else {
        return this.products.slice().sort(sortById);
      }
    },
  },
  mounted() {

    /*fetch('https://dummyjson.com/products')
      .then(function (response) {
        return response.json()
      })
      .then(function (data) {
        this.products = data.products
        console.log(products)
      })*/

    this.products =
      JSON.parse(localStorage.getItem("products")) || this.products;
  },
  watch: {
    products(products) {
      localStorage.setItem("products", JSON.stringify(products));
    },
  },
  computed: {
    addButtonActive: function () {
      return this.name && this.img && this.price;
    },
  },
  async created() {
    const response = await fetch("https://dummyjson.com/products");
    const data = await response.json();
    this.products = data.products;
    console.log(this.products)
  }
};
</script>


<style lang="scss">
@import "../index.scss";
.container {
  height: 100%;
}
.form {
  //background: aquamarine;
}
.products-container {
  //background: orange;
  .filters {
    //background: #7BAE73;
    height: 40px;
  }
  .products-grid {
    display: flex;
    flex-wrap: wrap;
    .product-container {
      padding: 5px;
      width: 100%;
      @media (min-width: $device-sm) {
        width: 48%;
      }
      @media (min-width: $device-md) {
        width: 32%;
      }
      .product {
        height: 423px;
        position: sticky;
        background: #fffefb;
        box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
          0px 6px 10px rgba(0, 0, 0, 0.02);
        border-radius: 10px;
        cursor: pointer;
      }
      .product:hover {
        box-shadow: 1px 1px 8px 1px #474747;
      }
    }
  }
}
/*//////////////////////////////////////*/
.forma {
  position: flex;
  width: auto;
  height: 440px;
  background: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
}
.h-add-product {
  /* Добавление товара */
  position: absolute;
  top: -0px;
  font-family: "Source Sans Pro";
  font-style: normal;
  font-weight: 600;
  font-size: 28px;
  color: #3f3f3f;
}
.h-name-product {
  /* Наименование товара */
  position: flex;
  width: 100px;
  height: 13px;
  left: 20px;
  top: 0px;
  font-family: "Source Sans Pro";
  font-style: normal;
  font-weight: 400;
  font-size: 10px;
  line-height: 13px;
  /* identical to box height */
  letter-spacing: -0.02em;
  /* Temp / Darks / Lesser */
  color: #49485e;
}
.nameproduct {
  /* Rectangle 28 */
  position: flex;
  width: 90%;
  height: 36px;
  left: 20px;
  top: 27px;
  /* Darks & Whites / White */
  background: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
}
.h-product-description {
  /* Описание товара */
  position: flex;
  width: 105px;
  height: 13px;
  left: 20px;
  top: 69px;
  font-family: "Source Sans Pro";
  font-style: normal;
  font-weight: 400;
  font-size: 10px;
  line-height: 13px;
  /* identical to box height */
  letter-spacing: -0.02em;
  /* Temp / Darks / Lesser */
  color: #49485e;
}
.h-enter-link {
  /*Ссылка на изображение товара */
  position: flex;
  width: 134px;
  height: 13px;
  left: 20px;
  top: 210px;
  font-family: "Source Sans Pro";
  font-style: normal;
  font-weight: 400;
  font-size: 10px;
  line-height: 13px;
  /* identical to box height */
  letter-spacing: -0.02em;
  /* Temp / Darks / Lesser */
  color: #49485e;
}
.enter-link {
  /* Rectangle 28 */
  position: flex;
  width: 90%;
  height: 36px;
  left: 20px;
  top: 237px;
  /* Darks & Whites / White */
  background: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
}
.h-price-product {
  /* Цена товара */
  position: flex;
  width: 59px;
  height: 13px;
  left: 20px;
  top: 279px;
  font-family: "Source Sans Pro";
  font-style: normal;
  font-weight: 400;
  font-size: 10px;
  line-height: 13px;
  /* identical to box height */
  letter-spacing: -0.02em;
  /* Temp / Darks / Lesser */
  color: #49485e;
}
.price-product {
  /* Rectangle 28 */
  position: flex;
  width: 90%;
  height: 36px;
  left: 20px;
  top: 306px;
  /* Darks & Whites / White */
  background: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
}
.product-description {
  /* Rectangle 28 */
  position: flex;
  width: 90%;
  height: 108px;
  left: 20px;
  top: 96px;
  /* Darks & Whites / White */
  background: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  resize: none;
}
.add-button {
  /* Rectangle 29 */
  position: flex;
  width: 90%;
  height: 36px;
  margin-top: 20px;
  background: #eeeeee;
  border-radius: 10px;
}
.add-button.active {
  position: flex;
  width: 90%;
  height: 36px;
  margin-top: 20px;
  background: #7bae73;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
}
.list {
  position: absolute;
  left: 380px;
  top: 75px;
  display: grid;
  grid-gap: 5px;
  grid-template-columns: repeat(auto-fit, 1432px);
  grid-template-rows: repeat(2, 332px);
}
/* Фильтры */
.filter {
  /* Rectangle 28 */
  position: relative;
  float: right;
  width: 121.49px;
  height: 36px;
  /* Darks & Whites / White */
  background: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
}
/* Фото товара */
.product-photo {
  display: block;
  position: relative;
  background: #fff;
  margin: 0 20px 20px 0;
  text-decoration: none;
  color: #474747;
  z-index: 0;
  width: 100%;
  height: 200px;
  border-radius: 10px;
}
.product-photo img {
  position: relative;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  max-width: 100%;
  max-height: 100%;
  margin: auto;
  transition: transform 0.4s ease-out;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}
/*
.product:hover .product-photo img {
	transform: scale(1.02);
}
*/
.h-description {
  position: relative;
  float: left;
  left: 15px;
  font-family: "Source Sans Pro";
  font-style: normal;
  font-weight: 600;
  color: #3f3f3f;
}
.description {
  position: relative;
  top: 30px;
  resize: none;
  width: 85%;
  height: 100px;
  background: white;
  border: none;
}
.price {
  position: absolute;
  float: left;
  left: 15px;
  top: 385px;
  font-size: 1em;
}
.rating {
  position: absolute;
  float: left;
  right: 15px;
  top: 385px;
  font-size: 1em;
}
.product-remove {
  opacity: 0;
  position: absolute;
  left: 92%;
  height: 25px;
  width: 25px;
  border-radius: 50%;
  border: 1px solid #000;
  background-image: url("data:image/jpeg;base64,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");
  background-position: left center;
  background-repeat: no-repeat;
  background-size: 100%;
}
.product:hover .product-remove {
  transform: translateY(0);
  opacity: 1;
  cursor: pointer;
}
p.required:after {
  color: red;
  content: " *";
}
.error {
  color: #fc2d2d;
}
</style>