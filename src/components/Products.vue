<template>
  <b-container>
    <h2 class="mt-4">Products</h2>
    <b-row>
      <b-col cols="11">
        <b-table outlined head-variant="gray" striped hover :items="items"></b-table>
      </b-col>
      <b-col cols="1" class="mt-5">
        <div v-for="button in items" :key="button">
          <b-button class="mt-2 px-3" variant="outline-dark">EDIT</b-button>
        </div>
      </b-col>
    </b-row>
    <b-row>
      <b-col md="3" offset-md="9">
        <b-button class="mt-2 px-3" variant="outline-dark">ADD PRODUCT</b-button>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      items: [
        {
          title: "Apple",
          description: "A sweet, edible fruit produced by an apple tree",
          price: 1.5
        },
        {
          title: "Banana",
          description:
            "An edible fruit - botanically a berry - produced by large flowering plants",
          price: 0.6
        }
      ]
    };
  }
};
</script>

<style>
</style>