<template>
  <ProductDisplay />
</template>

<script>
import ProductDisplay from './components/ProductDisplay.vue';
import './/assets/style/style.css';

export default {
  name: 'App',
  components: {
    ProductDisplay,
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
</style>
