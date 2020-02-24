<template>
  <b-container>
    <h2 class="my-3">Products</h2>
    <b-row>
      <b-col cols="10">
        <b-table outlined head-variant="gray" striped hover :items="products">
          <template v-slot:cell(price)="data">$ {{ data.value}}</template>
        </b-table>
      </b-col>
      <b-col cols="2" class="mt-5">
        <div v-for="product in products" :key="product.title">
          <b-button @click="editItem(product)" class="mt-2 px-3" variant="outline-dark">EDIT</b-button>
        </div>
      </b-col>
    </b-row>
    <b-row>
      <b-col md="3" offset-md="7" co>
        <b-button
          block
          @click="modalShow = !modalShow"
          class="mt-2 px-3"
          variant="outline-dark"
        >ADD PRODUCT</b-button>
        <b-modal v-model="modalShow" hide-footer :title="formTitle">
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

                <b-form-group label="Price ($)">
                  <b-form-input
                    id="input-live"
                    v-model="editedItem.price"
                    :state="nameState"
                    type="text"
                    required
                    placeholder="Enter Price"
                  ></b-form-input>
                  <b-form-invalid-feedback id="input-live-feedback">The price must be greater than 1</b-form-invalid-feedback>
                </b-form-group>

                <b-row>
                  <b-col md="6" offset-md="7">
                    <b-button type="reset" class="mx-1 px-3" variant="outline-secondary">RESET</b-button>
                    <b-button
                      type="submit"
                      class="mx-1 px-3"
                      v-if="nameState"
                      variant="outline-dark"
                    >SAVE</b-button>
                    <b-button
                      type="submit"
                      class="mx-1 px-3"
                      v-else
                      disabled
                      variant="outline-dark"
                    >SAVE</b-button>
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
    modalShow: false,
    products: [],
    editedIndex: -1,
    editedItem: {
      title: "",
      description: "",
      price: 0
    },
    defaultItem: {
      title: "",
      description: "",
      price: 0
    }
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    },
    nameState() {
      return this.editedItem.price > 0 ? true : false;
    }
  },

  watch: {
    modalShow(val) {
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
          price: 0.5
        }
      ];
    },

    showModal() {
      this.modalShow = true;
    },

    editItem(product) {
      this.showModal();
      this.editedIndex = this.products.indexOf(product);
      this.editedItem = Object.assign({}, product);
      this.dialog = true;
    },

    close() {
      this.modalShow = false;
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