<template>
  <div class="container">

    <div class="row">
      <div class="col d-flex justify-content-center align-items-center">
        <input v-model="new_product" type="text">
      </div>

      <div class="col d-flex justify-content-center align-items-center">
        <button class="btn btn-primary"
                v-on:click="create_new_product">
          Create Product
        </button>
      </div>
    </div>

    <div class="row">
      <div class = "product-warning" v-if="products.length === 0">
        <h1>Список порожній</h1>
      </div>

      <div v-if="products.length !== 0" class="col">
        <div class="products-container" >
          <div class = "product-wrap">
            <div class = "Title">
              <h1>Products for today </h1>
          <div class="product" v-for="product in products" :class="{ bought: product.bought }">
            <h1>Name:{{ product.text }}</h1>
            <h1>Date:{{ dateFormat(product.date) }}</h1>
            <button class = "btn btn-danger" @click="product.bought = true">Куплено</button>


            <!--            <div class = "button">-->
<!--                     <button class = "btn btn-danger" v-on:click="buy_product">Куплено</button>-->
<!--            </div>-->
          </div>
        </div>
          </div>
      </div>
        </div>
    </div>

  </div>
</template>


<script setup>
import {ref} from "vue";
const new_product =ref( "")
const products = ref ([])

function create_new_product(){
  let data = {
    date:Date.now(),
    text:new_product.value,
    bought:false
  }
  if (new_product.value.length !== 0 ){
    products.value.push(
        data
    )
    console.log(data)
    new_product.value = "";

  }
}
function dateFormat(itemstamp){
  const date = new Date(itemstamp);
  return date.toLocaleDateString("uk-UA");


}
function buy_product(){

}

</script>

<style scoped>
.products-container,.product-warning{
  podding-top:2rem 0;
  text-align: center;
  width: 100%;
  border:1px black dashed;
}
.product-wrap{
  border: 1px black dashed;
  border-radius: 10px;


}
.product {
  margin-top:1rem;
  box-shadow: 0 4px black;
  border-radius: 10px;

  display:flex;
  align-content:center;
  flex-wrap: wrap;
  justify-content:space-between;
}
.bought {
  background-color: #eee;
  color: #777;
  text-decoration: line-through;
}

</style>