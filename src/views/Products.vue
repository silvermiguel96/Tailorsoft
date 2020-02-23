<template>
  <b-container>
    <h2 class="mt-4">Products</h2>
    <b-row>
      <b-col cols="11">
        <b-table outlined head-variant="gray" striped hover :items="products"></b-table>
      </b-col>
      <b-col cols="1" class="mt-5">
        <div v-for="product in products" :key="product.title">
          <b-button @click="editItem(product)" class="mt-2 px-3" variant="outline-dark">EDIT</b-button>
        </div>
      </b-col>
    </b-row>
    <b-row>
      <b-col md="3" offset-md="9">
        <b-button
          id="show-btn"
          @click="showModal"
          class="mt-2 px-3"
          variant="outline-dark"
        >ADD PRODUCT</b-button>
        <b-modal ref="my-modal" hide-footer :title="formTitle">
          <b-card>
            <div>
              <b-form @submit="save" @reset="onReset">
                <b-form-group label="Product">
                  <b-form-input v-model="editedItem.title" required placeholder="Enter product"></b-form-input>
                </b-form-group>

                <b-form-group label="Description">
                  <b-form-input
                    v-model="editedItem.description"
                    required
                    placeholder="Enter description"
                  ></b-form-input>
                </b-form-group>

                <b-form-group label="Price">
                  <b-form-input
                    v-model="editedItem.price"
                    type="number"
                    required
                    placeholder="Enter Price"
                  ></b-form-input>
                </b-form-group>

                <b-row>
                  <b-col md="6" offset-md="7">
                    <b-button type="reset" class="mx-1 px-3" variant="outline-secondary">RESET</b-button>
                    <b-button type="submit" class="mx-1 px-3" variant="outline-dark">SAVE</b-button>
                  </b-col>
                </b-row>
              </b-form>
            </div>
          </b-card>
        </b-modal>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data: () => ({
    dialog: false,
    products: [],
    items: [],
    editedIndex: -1,
    editedItem: {
      title: "",
      description: 0,
      price: 0
    },
    defaultItem: {
      title: "",
      description: 0,
      price: 0
    }
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    }
  },

  watch: {
    dialog(val) {
      val || this.close();
    }
  },

  created() {
    this.initialize();
  },

  methods: {
    initialize() {
      this.products = [
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
      ];
    },
    showModal() {
      this.$refs["my-modal"].show();
    },
    editItem(product) {
      this.showModal();
      this.editedIndex = this.products.indexOf(product);
      this.editedItem = Object.assign({}, product);
      this.dialog = true;
    },

    close() {
      this.$refs["my-modal"].hide();
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      }, 300);
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.products[this.editedIndex], this.editedItem);
      } else {
        this.products.push(this.editedItem);
      }
      this.close();
    },
    onReset(evt) {
      evt.preventDefault();
      this.editedItem.title = "";
      this.editedItem.description = "";
      this.editedItem.price = 0;
    }
  }
};
</script>

<style>
</style>