<template>
  <div>
    <a-form @submit="onSubmit" >
      <card-order>
        <a-row v-if="rebuild">
          <a-col span="14">
            <select-custom
              :state-error="error_message.product == '' ? null : false"
              :message-error="error_message.product"
              v-model="add_product"
              :initial-value="add_product"
              placeholder="Selecione um produto"
              name="product"
              :options="products"
            />
          </a-col>
          <a-col span="10">
            <input-custom
              :state-error="error_message.quantity_validation"
              :message-error="error_message.quantity"
              v-model="add_quantity"
              :initial-value="add_quantity"
              name="quantity"
              placeholder="0"
            />
          </a-col>
        </a-row>
        <div v-else class="text-center">
          <a-spin style="width: 3rem; height: 3rem;" label="Loading..."></a-spin>
        </div>
        <template v-slot:footer>
          <a-button @click="add()" variant="success"> Adicionar Produto</a-button>
        </template>
      </card-order>
      <card-order-products-selected :data="products_selected" @remove="removeItem">
        <template v-slot:footer>
          <a-row>
            <a-col span="6">
              <a-button :disabled="products_selected.length == 0" html-type="submit" type="primary" variant="primary">Finalizar Compra</a-button>
            </a-col>
            <a-col span="4">
              <span class="h5">Totais:</span>
            </a-col>
            <a-col span="4">
              <span class="h5">Qtd Total: {{quantity}}</span>
            </a-col>
            <a-col span="6">
              <span class="h5">Valor Total: R$ {{total}}</span>
            </a-col>
            <a-col span="4">
              <a-button :disabled="products_selected.length == 0" @click="clear" variant="secondary">Limpar</a-button>
            </a-col>
          </a-row>
        </template>
        <template v-slot:description>
          <a-textarea
            id="textarea"
            v-model="form.description"
            placeholder="Descrição..."
            rows="3"
            max-rows="6"
          ></a-textarea>
        </template>
      </card-order-products-selected>
    </a-form>
  </div>
</template>

<script>
export default {

  props: {
    edit: {
      type: Boolean,
      default: false
    },
    title: {
      type: String,
      required: true
    },
    errorMessage: {
      type: Object | Array
    }
  },

  data() {
    return {
      form: {
        id:this.$route.params.edit || null,
        products:[],
        description:"",
        quantity:[]
      },
      products: [],
      rebuild: true,
      products_selected: [],
      add_product:"",
      add_quantity:"",
      message:"",
      error_message:[],
      categories:[],
      add_product_api:[],
      quantity:0,
      total:(0).toFixed(2)
    }
  },

  watch: {
    products_selected: function (val) {
      this.total = val.reduce((acc, {sub_total}) => {
        return acc + Number(sub_total)
      }, 0).toFixed(2);

      this.quantity = val.reduce((acc, {quantity}) => {
        return acc + Number(quantity)
      }, 0);

      this.form = {
        ...this.form,
        quantity: val.map(({quantity}) => quantity),
        products: val.map(({id}) => id)
      }
    },
  },

  mounted(){
    this.getProducts()
  },

  methods:{
    onSubmit(e){
      this.$nuxt.$loading.start()
      this.$emit("change", this.form);
      e.preventDefault();
    },

    removeItem(index){
      this.products_selected.splice(index,1)
    },

    getProducts(){
      this.$axios
        .$get(
          "product"
        )
        .then(({products}) => {
          this.products = products.map(({id, name, category_name, value}) => {
            return {value:id, title:name + ' - R$ ' +value }
          });
        })
    },

    setProduct(id){
      return this.$axios
        .$get(
          `product/`+id
        )
        .then(({product, success}) => {
          if(success == true)
          {
            const subtotal = this.add_quantity * product.value
            this.add_product_api = {
              id: product.id,
              name: product.name,
              quantity: this.add_quantity,
              value: parseFloat(product.value).toFixed(2),
              sub_total: subtotal.toFixed(2)
            }
          } else {
            this.message = message
            this.error_message = error_message
          }
        })
    },

    clear(e){
      this.form = {
        ...this.form,
        products: [],
        quantity: []
      }

      this.products_selected = []
    },

    rebuilder()
    {
      this.rebuild = false
      this.$nextTick(() => {
        this.rebuild = true
      })
    },

    async add(){
      let error_flag = false

      if(this.add_product == "")
      {
        error_flag = true
        this.error_message.product = "Informe um produto"
      } else
      {
        this.error_message.product = ""
      }

      if(this.add_quantity == "")
      {
        error_flag = true
        this.error_message.quantity = "Informe a quantidade"
        this.error_message.quantity_validation = false
      } else
      {
        this.error_message.quantity = ""
        this.error_message.quantity_validation = null
      }

      if(!error_flag)
      {
        await this.setProduct(this.add_product)

        await this.products_selected.push(this.add_product_api)

        this.add_product = ""
        this.add_quantity = ""
      }
      this.rebuilder()
    },
  }
}
</script>
