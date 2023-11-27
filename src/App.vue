<script >

export default {
  data() {
    return {
      products: [],
      filterdProducts: [],
      catagories: [],
      sortOrder: '',
      catagorieValue: '',
      searchValue: ''
    };
  },
  async mounted() {
    fetch('https://fakestoreapi.com/products/')
        .then((response) => response.json())
        .then((data) => {
          this.products = data
          this.filterdProducts = this.products
         this.products.forEach(e => {
            this.catagories.push(e.category)
         });
         this.catagories = new Set(this.catagories)
         this.catagories = [...this.catagories]
        });
  },
  methods :{
    searchProduct(){
      this.filterdProducts = this.products.filter((pr)=> pr.title == this.searchValue)
    },
    catagorieFilter(){
      this.filterdProducts = this.products.filter((pr)=> pr.category == this.catagorieValue)
    },
    sortPrice(){
      this.filterdProducts = this.filterdProducts.sort((a, b) => {
        if (this.sortOrder === 'low') 
          return a.price - b.price
           else  
          return b.price - a.price
      });
    },
    clear(){
      this.filterdProducts = this.products
      this.catagorieValue = ''
      this.searchValue = ''
      this.sortOrder = ''
    }
  },
};
</script>

<template>
  <div>
    <h1>Products</h1>
    <label for="">price</label>
    <select v-model="sortOrder" @change="sortPrice">
      <option value="low">Price: low to high</option>
      <option value="high">Price: high to low</option>
    </select>
    <label for="">catagories</label>
    <select v-model="catagorieValue" @change="catagorieFilter">
      <option v-for="product in catagories" :key="product">
       {{product}}
      </option>
    </select>
    <button @click="clear">x</button>
    <form @submit.prevent="searchProduct">
        <input type="text" v-model="searchValue" placeholder="write a product name" />
        <button type="submit">search</button>
      </form>
    <ul>
      <li v-for="product in filterdProducts" :key="product.id">
        <h2>{{ product.title }}</h2>
        <p>{{ product.description }}</p>
        <p>{{ product.price+'$'}}</p>
      </li>
    </ul>
  </div>
</template>

<style scoped>
ul{
  width: 50%;
  margin-left: auto;
  margin-right: auto;
  list-style: none;
}

button{
  margin-left: 1rem;
}
input,select{
  height: 2rem;
  margin: 0 1rem;
}
</style>
