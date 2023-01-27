<template>
  <div :class="!dataMenWomen ? 'bg-grey' : productAPI.category === 'men\'s clothing' ? 'bg-men' : 'bg-women'" class="container" v-if="setLoading === false">
    <div v-if="dataMenWomen === true" class="product-display">
      <div class="image-section">
        <img class="product-image" :src="productAPI.image" alt="image-product" />
      </div>
      <div class="product-description">
        <h1 :class="productAPI.category === 'men\'s clothing' ? 'text-blue' : 'text-maroon'" class="product-title">{{ productAPI.title }}</h1>
        <div class="category-section">
          <p class="category">{{ productAPI.category }}</p>
          <div class="rating">
            <p class="rating-text">{{ rate.rate }}</p>
            <div class="rating-circle">
              <div :class="productAPI.category === 'men\'s clothing' ? 'color-blue' : 'color-maroon'" class="circle"></div>
              <div :class="productAPI.category === 'men\'s clothing' ? 'color-blue' : 'color-maroon'" class="circle"></div>
              <div :class="productAPI.category === 'men\'s clothing' ? 'color-blue' : 'color-maroon'" class="circle"></div>
              <div :class="productAPI.category === 'men\'s clothing' ? 'color-blue' : 'color-maroon'" class="circle"></div>
              <div :class="productAPI.category === 'men\'s clothing' ? 'color-blue' : 'color-maroon'" class="circle"></div>
            </div>
          </div>
        </div>
        <hr />
        <p class="description">
          {{ productAPI.description }}
        </p>
        <hr />
        <div :class="productAPI.category === 'men\'s clothing' ? 'text-blue' : 'text-maroon'" class="price">${{ productAPI.price }}</div>
        <div class="buttons">
          <button :class="productAPI.category === 'men\'s clothing' ? 'buy-button-men' : 'buy-button-women'" class="buy-button">Buy Product</button>
          <button @click="getProduct()" :class="productAPI.category === 'men\'s clothing' ? 'next-button-men' : 'next-button-women'" class="next-button">Next Product</button>
        </div>
      </div>
    </div>
    <div v-else :class="dataMenWomen === true ? 'return' : 'bg-grey'" class="container">
      <div class="product-unavailable">
        <p class="unavailable-text">This Product is unavailable to show</p>
        <button @click="getProduct()" class="unavailable-button">Next Product</button>
      </div>
    </div>
  </div>
  <div v-else-if="setLoading === true">
    <div id="loader">
      <div id="spinner-back"></div>
      <div id="spinner"></div>
    </div>
  </div>
</template>

<script>
import '../assets/style/style.css';

export default {
  name: 'ProductDisplay',
  data() {
    return {
      dataIndex: 0,
      setLoading: false,
      productAPI: {},
      rate: {},
      dataMenWomen: false,
    };
  },
  components: {},
  methods: {
    async getDataFromAPI() {
      const api = await fetch(`https://fakestoreapi.com/products/${this.dataIndex}`);
      const response = await api.json();
      return response;
    },

    async getProduct() {
      this.setLoading = true;

      if (this.dataIndex !== 20) {
        this.dataIndex++;
      } else {
        this.dataIndex = 1;
      }

      let dataProduct = await this.getDataFromAPI();
      if (dataProduct.category === "men's clothing" || dataProduct.category === "women's clothing") {
        this.dataMenWomen = true;
        this.productAPI = dataProduct;
        this.rate = dataProduct.rating;
      } else {
        this.dataMenWomen = false;
      }
      this.setLoading = false;
    },
  },
  mounted() {
    this.getProduct();
  },
};
</script>

<style>
:root {
  --dark-blue: #002772;
  --pink: #fde2ff;
  --black: #1e1e1e;
  --maroon: #720060;
  --light-blue: #d6e6ff;
  --dark-grey: #3f3f3f;
  --light-grey: #dcdcdc;
  --white: #ffffff;
}

.container {
  display: grid;
  align-items: center;
  height: 100vh;
}

.bg-men {
  background: linear-gradient(180deg, #d6e6ff 70%, #ffffff 30%);
}

.bg-women {
  background: linear-gradient(180deg, #fde2ff 70%, #ffffff 30%);
}

.bg-grey {
  background: linear-gradient(180deg, #dcdcdc 70%, #ffffff 30%);
}

.product-display {
  display: flex;
  position: relative;
  left: 150px;
  padding: 3.5em;
  margin-top: 50px;
  margin-bottom: 40px;
  justify-content: center;
  width: 70vw;
  column-gap: 2em;
  background-color: var(--white);
  border-radius: 8px;
  box-shadow: 12px 9px 26px -1px rgba(0, 0, 0, 0.75);
  -webkit-box-shadow: 12px 9px 26px -1px rgba(0, 0, 0, 0.75);
  -moz-box-shadow: 12px 9px 26px -1px rgba(0, 0, 0, 0.75);
}

.product-unavailable {
  display: grid;
  align-items: center;
  row-gap: 0;
  position: relative;
  left: 150px;
  top: 50px;
  padding: 3.5em;
  justify-content: center;
  width: 70vw;
  height: 50vh;
  column-gap: 2em;
  background-color: var(--white);
  border-radius: 8px;
  box-shadow: 12px 9px 26px -1px rgba(0, 0, 0, 0.75);
  -webkit-box-shadow: 12px 9px 26px -1px rgba(0, 0, 0, 0.75);
  -moz-box-shadow: 12px 9px 26px -1px rgba(0, 0, 0, 0.75);
  background-image: url(../assets/sad-face.png);
}

.text-blue {
  color: var(--dark-blue);
}

.text-maroon {
  color: var(--maroon);
}

.product-description > * {
  margin-bottom: 0.8em;
}

.category-section {
  display: flex;
  justify-content: space-between;
}

.product-image {
  width: 250px;
}

.next-button-men {
  background-color: var(--white);
  color: var(--dark-blue);
  border: 2px solid var(--dark-blue);
}

.next-button-women {
  background-color: var(--white);
  color: var(--maroon);
  border: 2px solid var(--maroon);
}

.category {
  font-size: 16px;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: 400;
  color: var(--dark-grey);
}

.unavailable-text {
  font-size: 18px;
  margin: auto;
  position: relative;
  top: 50px;
  font-weight: 500;
  font-family: Arial, Helvetica, sans-serif;
}

.rating {
  display: flex;
}

.rating-text {
  font-size: 16px;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: 400;
  color: var(--dark-grey);
}

.rating-circle {
  margin-left: 14px;
  display: flex;
  gap: 1px;
}

.circle {
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background-color: var(--white);
}

.color-blue {
  background-color: var(--dark-blue);
}

.color-maroon {
  background-color: var(--maroon);
}

.description {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 20px;
  height: 200px;
}

.product-title {
  font-size: 24px;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: 600;
}

.buttons {
  display: flex;
  justify-content: space-between;
}

.buy-button {
  width: 45%;
  height: 30px;
  font-size: 18px;
  border: none;
  font-weight: 500;
}

.buy-button-men {
  background-color: var(--dark-blue);
  color: var(--white);
}

.buy-button-women {
  background-color: var(--maroon);
  color: var(--white);
}

.unavailable-button {
  width: 300px;
  height: 40px;
  font-size: 18px;
  font-weight: 500;
  color: black;
  border: 2px solid black;
  background-color: var(--white);
  position: relative;
  bottom: 50px;
}

.next-button {
  width: 45%;
  height: 30px;
  font-size: 18px;

  font-weight: 500;
}

.price {
  font-size: 20px;
  font-family: Arial, Helvetica, sans-serif;
}

#loader {
  position: relative;
  margin: auto;
  bottom: 100px;

  /*control for size*/
  --size: 128px;

  --width: 800px;
  --thickness: calc(var(--width) / 8);

  /*control for duration of 1 loop*/
  --duration: 1.5s;

  width: var(--width);
  aspect-ratio: 1/1;

  transform: scale(0.16);
}

#spinner-back {
  position: absolute;

  width: var(--width);
  aspect-ratio: 1/1;

  border-radius: var(--width);

  border: var(--thickness) solid #0099f0;
}

#spinner {
  position: absolute;
  top: 0;
  left: 0;
  width: var(--width);
  aspect-ratio: 1/1;

  border-radius: var(--width);

  border: var(--thickness) solid #b3e0fb;
  border-right-color: transparent;

  transform-origin: 50% 50%;

  animation: spinner var(--duration) infinite ease-in-out;
}

#spinner::before,
#spinner::after {
  content: '';

  position: absolute;
  height: calc(var(--thickness));
  aspect-ratio: 1/1;

  top: calc(var(--thickness) * 0.4);
  right: calc(var(--thickness) * 0.2);

  background-color: #0099f0;

  border-radius: var(--thickness);
}

#spinner::after {
  top: unset;
  bottom: calc(var(--thickness) * 0.4);
}

@keyframes spinner {
  68% {
    transform: rotatez(2.63turn);
  }

  96%,
  100% {
    transform: rotatez(2turn);
  }
}
</style>
