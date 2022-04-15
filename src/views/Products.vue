<template>
  <div>
    <h1>Products</h1>
    <input id="asd1" type="num" placeholder="минимальная цена">
    <input id="asd2" type="num" placeholder="максимальная цена">
    <button @click="asd()">сортировать</button>
    <div v-for="p in Products" :key="p.id" class="cart">
      <p>{{ p.title }}</p>
      <img :src="'http://localhost:8055/assets/'+ p.Image.id" alt="" style="width: 400px;"/>
      <p>{{ p.description }}</p>
      <p>{{ p.price }}</p>
      <button @click="move(p.id)">click</button>
    </div>
  </div>
</template>

<script>
import gql from "graphql-tag";
export default {
  apollo: {
    Products: {
      query: gql`
        query products($limit: Int! $sort: [String!]! $gte: Float!, $lte: Float!) {
          Products(limit: $limit sort: $sort filter: { _and: [{ price: { _gte: $gte } }, { price: { _lte: $lte } }] }) {
            id
            title
            description
            price
            Image {
            id
            }
          }
        }`,
      variables() {
        return {
          limit: this.limit,
          sort: this.sort,
          gte: Number(this.num1),
          lte: Number(this.num2),
        };
      },
    },
  },
  data() {
    return {
      limit: 10,
      sort: ["price"],
      num1: 0,
      num2: 10000,
    }
  },
  methods: {
    move(id) {
      this.$router.push("/products/" + id);
    },
    asd() {
      this.num1 = document.getElementById('asd1').value;
      this.num2 = document.getElementById('asd2').value;
    }
    },
};
</script>

<style scoped>
  .cart {
    border: 1px solid black;
    padding: 10px;
  }
</style>