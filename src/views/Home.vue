<template>
  <div class="container">

    <div class="row">
      <div class="col d-flex gap-3 justify-content-center align-items-center mt-4">
        <input v-model="new_product" type="text" class="form-control w-25" placeholder="Назва товару">
        <input v-model="price" type="number" class="form-control w-25" placeholder="Ціна">
      </div>

      <div class="col d-flex justify-content-center align-items-center mt-4">
        <button class="btn btn-primary px-4"
                v-on:click="create_new_product">
          Додати товар
        </button>
      </div>
    </div>

    <div class="row mt-4">

      <div class="product-warning" v-if="products.length === 0">
        <h2 class="text-center text-secondary">Список порожній</h2>
      </div>

      <div v-if="products.length !== 0 && filtered_products.length === 0">
        <h2 class="text-center text-secondary">Немає покупок для відображення</h2>
      </div>

      <div v-if="products.length !== 0 && filtered_products.length > 0" class="col">
        <div class="products-container p-3">
          <h2 class="text-center mb-4">Products for today</h2>

          <div class="product card p-3"
               v-for="product in filtered_products"
               :class="{ bought: product.bought }">

            <div class="d-flex justify-content-between w-100 align-items-center">
              <div>
                <h4 class="mb-1">{{ product.text }}</h4>
                <p class="text-muted m-0">
                  Дата покупки: <strong>{{ dateFormat(product.date) }}</strong>
                </p>
                <p class="m-0">Ціна: <strong>{{ product.price }} грн</strong></p>
              </div>

              <div class="d-flex flex-column gap-2">
                <button class="btn btn-success btn-sm"
                        @click="markasBought(product)">
                  Куплено
                </button>

                <button class="btn btn-danger btn-sm"
                        @click="delete_product(product)">
                  Видалити
                </button>
              </div>
            </div>

          </div>

        </div>
      </div>

    </div>

    <div class="row">
      <div class="col mt-4 text-center">
        <h3 class="sum-of-products">
          Сума всіх куплених товарів: <strong>{{ total }}</strong> грн
        </h3>
      </div>
    </div>

    <div class="row mt-3">
      <div class="col text-center">
        <button class="btn btn-outline-primary mx-2" @click="filter = 'all'">Усі</button>
        <button class="btn btn-outline-success mx-2" @click="filter = 'bought'">Куплені</button>
        <button class="btn btn-outline-warning mx-2" @click="filter = 'notBought'">Не куплені</button>
      </div>
    </div>

  </div>
</template>


<script setup>
import {ref, computed} from "vue";

const new_product = ref("");
const products = ref([]);
const price = ref(0);

function create_new_product() {
  let data = {
    date: null,
    text: new_product.value,
    price: price.value,
    bought: false
  }

  if (new_product.value.length !== 0) {
    products.value.push(data)
    new_product.value = "";
    price.value = 0;
  }
}

function dateFormat(timestamp) {
  if (!timestamp) return "-";
  return new Date(timestamp).toLocaleString("uk-UA");
}

function markasBought(product) {
  product.bought = true;

  product.date = new Date().toLocaleString("uk-UA");
}

function delete_product(product) {
  products.value = products.value.filter(item => item !== product);
}

const total = computed(() =>
    products.value
        .filter(p => p.bought)
        .reduce((sum, p) => sum + (p.price || 0), 0)
);

const filter = ref("all");

const filtered_products = computed(() => {
  if (filter.value === "bought") return products.value.filter(p => p.bought);
  if (filter.value === "notBought") return products.value.filter(p => !p.bought);
  return products.value;
});
</script>


<style scoped>
.products-container {
  border: 1px solid #ddd;
  border-radius: 12px;
  background: #fafafa;
}

.product {
  margin-top: 1rem;
  border-radius: 12px;
  transition: 0.25s;
}

.product:hover {
  transform: scale(1.02);
  box-shadow: 0 4px 15px rgba(0,0,0,0.15);
}

.bought {
  background-color: #e9ecef !important;
  color: #888;
  text-decoration: line-through;
}

.sum-of-products {
  color: #6a0dad;
  font-weight: bold;
}

input {
  padding: 10px;
  border-radius: 8px !important;
}

button {
  min-width: 110px;
}
</style>
