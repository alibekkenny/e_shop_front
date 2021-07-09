<template>
  <div class="bg-gray-100">
    <div class="px-4 py-3 w-full inline-block bg-blue-500 text-white">
      <img
        width="60 px"
        class="rounded-full inline-block"
        src="https://avatanplus.com/files/resources/original/58cd68232aa0815ae25ec967.png"
        alt="logotype"
      />
      <span class="inline-block">Soon</span>
    </div>

    <div class="w-full px-10 mt-3">
      <label class="w-full inline-block font-bold">Цена</label>
      <div class="py-2 inline-block">
        <input
          type="text"
          class="inline-block rounded px-2 py-1 w-20 border border-gray-200"
          v-model="input_from"
        />
        <label> - </label>
        <input
          type="text"
          class="inline-block rounded px-2 py-1 w-20"
          v-model="input_to"
        />
        <button
          @click="filterQuery()"
          class="
            ml-2
            bg-blue-500
            text-white
            px-5
            py-1
            rounded
            float-right
            font-bold
          "
        >
          Найти
        </button>
      </div>
    </div>
    <div class="w-full px-10">
      <div class="bg-white p-5">
        <span class="text-gray-700">Смартфоны</span>
        <div class="mt-2">
          <label class="inline-flex items-center">
            <input
              type="radio"
              class="form-radio"
              name="accountType"
              value="all"
              id="all"
              checked
            />
            <span class="ml-2">Все</span>
          </label>
          <label class="inline-flex items-center ml-6">
            <input
              type="radio"
              class="form-radio"
              name="accountType"
              value="apple"
              id="apple"
            />
            <span class="ml-2">Apple iPhone</span>
          </label>
          <label class="inline-flex items-center ml-6">
            <input
              type="radio"
              class="form-radio"
              name="accountType"
              value="samsung"
              id="samsung"
            />
            <span class="ml-2">Samsung Galaxy</span>
          </label>
          <label class="inline-flex items-center ml-6">
            <input
              type="radio"
              class="form-radio"
              name="accountType"
              value="xiaomi"
              id="xiaomi"
            />
            <span class="ml-2">Xiaomi</span>
          </label>
          <label class="inline-flex items-center ml-6">
            <input
              type="radio"
              class="form-radio"
              name="accountType"
              value="huawei"
              id="huawei"
            />
            <span class="ml-2">Huawei</span>
          </label>
        </div>
      </div>
    </div>
    <div class="px-10 py-4 flex w-full">
      <div class="relative text-gray-600">
        <input
          type="search"
          name="search"
          placeholder="Поиск"
          class="bg-white h-10 px-5 pr-10 rounded-full"
          v-model="searchbar"
        />
        <button @click="filterQuery()" class="absolute right-0 top-0 mt-3 mr-4">
          <svg
            class="h-4 w-4 fill-current"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            version="1.1"
            id="Capa_1"
            x="0px"
            y="0px"
            viewBox="0 0 56.966 56.966"
            style="enable-background: new 0 0 56.966 56.966"
            xml:space="preserve"
            width="512px"
            height="512px"
          >
            <path
              d="M55.146,51.887L41.588,37.786c3.486-4.144,5.396-9.358,5.396-14.786c0-12.682-10.318-23-23-23s-23,10.318-23,23  s10.318,23,23,23c4.761,0,9.298-1.436,13.177-4.162l13.661,14.208c0.571,0.593,1.339,0.92,2.162,0.92  c0.779,0,1.518-0.297,2.079-0.837C56.255,54.982,56.293,53.08,55.146,51.887z M23.984,6c9.374,0,17,7.626,17,17s-7.626,17-17,17  s-17-7.626-17-17S14.61,6,23.984,6z"
            />
          </svg>
        </button>
      </div>
      <div class="w-full inline-block">
        <div class="float-right">
          <select
            @change="changed()"
            class="form-select px-4 py-3 rounded-full bg-white"
            v-model="key"
          >
            <option value="1">По умолчанию</option>
            <option value="2">По убыванию</option>
            <option value="3">По возрастанию</option>
          </select>
        </div>
      </div>
    </div>
    <div
      class="
        p-10
        grid grid-cols-1
        sm:grid-cols-1
        md:grid-cols-3
        lg:grid-cols-4
        xl:grid-cols-4
        gap-5
      "
    >
      <!--Card 1-->
      <div
        v-for="product in products"
        :key="product.id"
        class="bg-white rounded overflow-hidden shadow-lg"
      >
        <img class="w-full" :src="product.image_path" alt="Phone image" />
        <div class="px-6 py-4">
          <div class="font-bold text-xl mb-2">{{ product.title }}</div>
          <p class="text-gray-700 text-base">Цена: ${{ product.price }}</p>
        </div>
        <button
          class="
            relative
            bg-blue-500
            text-white
            px-5
            py-2
            float-right
            rounded
            font-bold
            overflow-hidden
            mr-4
            mb-3
          "
        >
          Подробнее
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import gql from "graphql-tag";
export default {
  data() {
    return {
      products: [],
      errors: [],
      key: 1,
      input_from: {},
      input_to: {},
      min: {},
      max: {},
      searchbar: "",
    };
  },
  mounted() {
    this.sortProductsByDefault();
    this.declareMinMax();
  },
  apollo: {
    products: gql`
      {
        products {
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
  },
  methods: {
    declareMinMax() {
      let prices = [];
      this.products.forEach((x) => prices.push(x.price));
      this.min = Math.min.apply(null, prices);
      this.max = Math.max.apply(null, prices);
      this.input_from = this.min;
      this.input_to = this.max;
    },
    sortProductsByAsc() {
      this.products.sort((a, b) => (a.price > b.price ? 1 : -1));
    },
    sortProductsByDesc() {
      this.products.sort((a, b) => (a.price < b.price ? 1 : -1));
    },
    sortProductsByDefault() {
      this.products.sort((a, b) => (a.title > b.title ? 1 : -1));
    },
    async filterQuery() {
      let company;
      if (document.getElementById("apple").checked) {
        company = "apple";
      }
      if (document.getElementById("samsung").checked) {
        company = "samsung";
      }
      if (document.getElementById("xiaomi").checked) {
        company = "xiaomi";
      }
      if (document.getElementById("huawei").checked) {
        company = "huawei";
      }
      if (document.getElementById("all").checked) {
        company = "";
      }
      this.input_from = this.input_from ? Number(this.input_from) : this.min;
      this.input_to = this.input_to ? Number(this.input_to) : this.max;
      const res = await this.$apollo.query({
        query: gql`
          query ($from: Int, $to: Int, $company: String, $search: String) {
            products(
              filter: {
                price: { gte: $from, lte: $to }
                company: { contains: $company }
                title: { contains: $search }
              }
            ) {
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
        variables: {
          from: this.input_from,
          to: this.input_to,
          company: company,
          search: this.searchbar,
        },
      });
      this.products = res.data.products;
      this.changed();
    },
    changed() {
      if (this.key == 1) {
        this.sortProductsByDefault();
      } else if (this.key == 2) {
        this.sortProductsByDesc();
      } else if (this.key == 3) {
        this.sortProductsByAsc();
      }
    },
  },
};
</script>
