  <template>
      <div class="row">
        <div class="col-md-12">
          <div class="card">
            <!-- <paper-table :title="table1.title" :sub-title="table1.subTitle" :data="products" :columns="tableColumns"> -->

            <!-- </paper-table> -->
          </div>
        </div>
        <input type="text" v-model= "product.id"> ID <br>
        <input type="text" v-model= "product.name"> Name <br>
        <input type="text" v-model= "product.price"> Price <br>
          <button v-on:click= "AddToApi"> add product</button>
          <button v-on:click= "updateProds"> update product</button>
          <table class="table table-striped table-borders">
          <thead>
          <tr>
          <th class="center"> ID </th>
          <th class="center"> Name </th>
          <th class="center"> Price </th>
          <th class="center"> Created at </th>
          <th class="center"> Updated at </th>
          <th class="center"> Seller Name </th>
          <th class="center"> Action </th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="(prods, index) of products.data">
          <td class="test-left" v-model= "current_id=prods._id"> {{ prods._id }} </td>
          <td class="test-left"> {{ prods.name }} </td>
          <td class="test-left"> {{ prods.price }} </td>
          <td class="test-left"> {{ prods.createdAt }} </td>
          <td class="test-left"> {{ prods.updatedAt }} </td>
          <td class="test-left"> {{prods.seller}} </td>
          <td class="text-center">
          <button  type="button" class='btn btn-info btn-xs'>
          <span class="glyphicon glyphicon-wrench"> </span> Edit</button>
          <button class="btn btn-danger btn-xs" v-on:click="deleteProd">
          <span class="glyphicon glyphicon-minus"> </span> Delete</button></td>
          </tr>
          </tbody>
          </table>
          <p> {{product.id}} </p>
      </div>
  </template>
  <script>

import axios from 'axios'
import PaperTable from 'components/UIComponents/PaperTable.vue'
const tableColumns = ['Id', 'Name', 'Price', 'CreatedAt', 'UpdatedAt', 'SellerName']
  /*
  const tableData = [{
    id: 1,
    name: 'Dakota Rice',
    price: '$36.738',
    createdat: '15/2/2018',
    updatedat: '16/2/2018',
    sellername: 'Mo'
  },
  {
    id: 2,
    name: 'Minerva Hooper',
    price: '$36.738',
    createdat: '15/2/2018',
    updatedat: '16/2/2018',
    sellername: 'Mo'
  }]
  */

export default {
  components: {
    PaperTable
  },
  data () {
    return {
      product: {
        name: '',
        price: '',
        createdat: '',
        updatedat: '',
        id: ''
      },
      products: [],
      current_id: '',
      test: ''
      // table1: {
      //   title: 'Stripped Table',
      //   subTitle: 'Here is a subtitle for this table',
      //   columns: [...tableColumns],
      //   data: [...products]
      // }
    }
  },
  Subscriptions () {
    return {
      // products: []
    }
  },
  // mounted () {
  //   this.getProducts()
  // },
  created () {
    axios.get('http://localhost:3000/api/product/getProducts').then((response) => {
      // console.log(response.data)
      this.products = response.data
      console.log(this.products.data)
    })
  },
  methods: {
    AddToApi () {
      axios.post('http://localhost:3000/api/product/createProduct', {
        name: this.product.name,
        price: this.product.price
      })
      .then((response) => {
        console.log(response)
        window.location.reload()
      })
      .catch((error) => {
        console.log(error)
      })
    },
    deleteProd: function () {
      axios.delete('http://localhost:3000/api/product/deleteProduct/' + this.current_id).then((response) => {
        console.log(response)
        window.location.reload()
      })
    },
    updateProds: function () {
      axios.patch('http://localhost:3000/api/product/updateProduct/' + this.product.id, {
        name: this.product.name,
        price: this.product.price
      }).then((response) => {
        console.log(response)
        window.location.reload()
      })
    }
    // getProducts () {
    //   axios.get('http://localhost:3000/api/product/getProducts').then((response) => {
    //   // console.log(response.data)
    //     this.products = response.data
    //     console.log(this.products)
    //   })
    // }
  }
}
  </script>
  <style>

  </style>
