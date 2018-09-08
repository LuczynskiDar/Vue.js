<!--10- 1. There's a <template> part -->

<template>
  <div id="app">
    <h2>My awesome list</h2>
    <!-- 3. Then we can use component by kebab-cased name and providing :input -->
    <product-list :products="products"></product-list>

    <form @submit.prevent="onSubmit()">
      <!-- 2. Any Angular fan here? v-model makes a binding with given object -->
      <!-- <input v-model="newProduct.name"> -->

      <input
        name="product"
        v-model="newProduct.name"
        v-validate="'required|min:3'"
      >

      <button>Add</button>
      <!--3- 2. errors are added by default when validation is initialized and have some useful methods -->
      <div v-show="errors.has('product')">
        {{ errors.first('product') }}
      </div>

    </form>
  </div>
</template>

<script>
import uuid from 'uuid/v4';
import ProductList from '../components/ProductList'

export default {
  name: 'app',
  
  components: {
    ProductList  
    },

  data() {
    return {
      products: [{
        id: 0,
        name: 'Coffee'
      }, {
        id: 1,
        name: 'Pizza'
      }],
            //3/ 3. Here goes the definition
      newProduct: {
        name: ''
      }
    }
  },
  methods: {
    removeLast() {
      this.products.pop();
    },

    onSubmit() {
      // 3. On the JS side we need to use yet another injected value called $validator to validate all the fields
      this.$validator.validateAll().then(result => {
        if (!result) {
          return;
        }
        this.products.push({
          id: uuid(),
          ...this.newProduct
        });
        this.newProduct.name = '';
        // 4/ and reset validation state after adding a product
        this.$validator.reset();
      });
    }

  }
    
}
</script>

<!--9- 3. And <style> part -->
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>