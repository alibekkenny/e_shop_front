<template>
    <div class="bg-gray-100 h-screen">
    <div class="px-4 py-3 w-full inline-block bg-blue-500 text-white">
      <img
        width="60 px"
        class="rounded-full inline-block"
        src="https://avatanplus.com/files/resources/original/58cd68232aa0815ae25ec967.png"
        alt="logotype"
      />
      <span class="inline-block">Soon</span>
    </div>

    <div class="w-full px-10 py-16 flex">
      <img class="inline-block w-3/5"
        :src="this.product.image_path"
        alt="product image"/>
        <div class="bg-white px-10 py-10 ml-10 inline-block w-full text-xl">
            <div class="flex mb-7">
                <h1 class="flex-auto text-3xl font-semibold inline-block"> {{this.product.title}} </h1>
            </div>
            <div class="mb-2"> <span class="font-semibold">Описание: </span> {{this.product.description}}</div>
            <div class="mb-2"><span class="font-semibold">Производитель:</span> {{this.product.company}}</div>
            <div class="mb-2"><span class="font-semibold">Память:</span> {{this.product.storage}} GB</div>
            <div class="mb-2 font-semibold" >Цена: <span class="font-semibold text-gray-500"> ${{this.product.price}}</span> </div>
            <nuxt-link to="/" ><button class="mt-80 bg-blue-500 w-1/5 h-10 rounded text-white float-right">Назад </button></nuxt-link>
        </div>
    </div>
  </div>
</template>
<script>
import gql from "graphql-tag";
export default {
  data() {
    return {
      product: "",
      errors: [],
    };
  },
  apollo: {
    product: {
      query: gql`
        query ($id: Int!) {
          product(id: $id) {
            id
            title 
            description
            price
            company
            storage
            image_path
          }
        }
      `,
      variables() {
        return {
          id: Number(this.$route.params.id),
        };
      },
    },
  },
  methods: {
    
  },
};
</script>