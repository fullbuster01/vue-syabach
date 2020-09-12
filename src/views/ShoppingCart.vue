<template>
  <div>
    <HeaderShayna />
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more text-left">
              <router-link to="/">
                <i class="fa fa-home"></i> Home
              </router-link>
              <span>Shopping Cart</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
      <div class="container">
        <div class="row">
          <div class="col-lg-8">
            <div class="row">
              <div class="col-lg-12">
                <div class="cart-table">
                  <table>
                    <thead>
                      <tr>
                        <th>Image</th>
                        <th class="p-name text-center">Product Name</th>
                        <th>Price</th>
                        <th>Action</th>
                      </tr>
                    </thead>
                    <tbody v-if="keranjangUser && keranjangUser.length > 0">
                      <tr v-for="keranjang in keranjangUser" :key="keranjang.id">
                        <td class="cart-pic first-row">
                          <img class="img-cart" :src="keranjang.photo" />
                        </td>
                        <td class="cart-title first-row text-center">
                          <h5>{{ keranjang.name }}</h5>
                        </td>
                        <td class="p-price first-row">Rp. {{ keranjang.price }}</td>
                        <td @click="removeItem(keranjang.id)" class="si-close">
                              <i class="ti-close"></i>
                            </td>
                      </tr>
                      <!-- <tr>
                        <td class="cart-pic first-row">
                          <img src="img/cart-page/product-1.jpg" />
                        </td>
                        <td class="cart-title first-row text-center">
                          <h5>Pure Pineapple</h5>
                        </td>
                        <td class="p-price first-row">$60.00</td>
                        <td class="delete-item">
                          <a href="#">
                            <i class="material-icons">close</i>
                          </a>
                        </td>
                      </tr> -->
                    </tbody>
                    <tbody v-else>
                          <tr>
                            <td colspan="4">Keranjang Kosong.</td>
                          </tr>
                        </tbody>
                  </table>
                </div>
              </div>
              <div class="col-lg-8">
                <h4 class="mb-4 text-left">Informasi Pembeli:</h4>
                <div class="user-checkout text-left">
                  <form>
                    <div class="form-group">
                      <label for="namaLengkap">Nama lengkap</label>
                      <input
                        type="text"
                        v-model="customerInfo.name"
                        class="form-control"
                        id="namaLengkap"
                        aria-describedby="namaHelp"
                        placeholder="Masukan Nama"
                      />
                    </div>
                    <div class="form-group">
                      <label for="emailAddress">Email Address</label>
                      <input
                        type="email"
                        v-model="customerInfo.email"
                        class="form-control"
                        id="emailAddress"
                        aria-describedby="emailHelp"
                        placeholder="Masukan Email"
                      />
                    </div>
                    <div class="form-group">
                      <label for="noHP">No. HP</label>
                      <input
                        type="text"
                        v-model="customerInfo.number"
                        class="form-control"
                        id="noHP"
                        aria-describedby="noHPHelp"
                        placeholder="Masukan No. HP"
                      />
                    </div>
                    <div class="form-group">
                      <label for="alamatLengkap">Alamat Lengkap</label>
                      <textarea v-model="customerInfo.address" class="form-control" id="alamatLengkap" rows="3"></textarea>
                    </div>
                  </form>
                </div>
              </div>
            </div>
          </div>
          <div class="col-lg-4">
            <div class="row">
              <div class="col-lg-12">
                <div class="proceed-checkout text-left">
                  <ul>
                    <li class="subtotal">
                      ID Transaction
                      <span>#SH12000</span>
                    </li>
                    <li class="subtotal mt-3">
                      Subtotal
                      <span>Rp. {{ subHarga }}</span>
                    </li>
                    <li class="subtotal mt-3">
                      Pajak
                      <span>10% = Rp. {{ totalPajak }}</span>
                    </li>
                    <li class="subtotal mt-3">
                      Total Biaya
                      <span>Rp. {{ totalHarga }}</span>
                    </li>
                    <li class="subtotal mt-3">
                      Bank Transfer
                      <span>Mandiri</span>
                    </li>
                    <li class="subtotal mt-3">
                      No. Rekening
                      <span>2208 1996 1403</span>
                    </li>
                    <li class="subtotal mt-3">
                      Nama Penerima
                      <span>Shayna</span>
                    </li>
                  </ul>
                  <!-- <router-link to="/success" class="proceed-btn">I ALREADY PAID</router-link> -->
                  <a @click="checkout()" href="#" class="proceed-btn">I ALREADY PAID</a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Shopping Cart Section End -->
  </div>
</template>

<script>
import HeaderShayna from "@/components/HeaderShayna.vue";

import axios from "axios";


export default {
  name: "ShoppingCart",
  components: {
    HeaderShayna
  },
  data() {
    return {
      keranjangUser: [],
      customerInfo:{
        name: '',
        email: '',
        number: '',
        address: ''
      }
    };
  },
  methods: {
    removeItem(idx){
      // cari tahu id dari item yang akan dihapus
      let keranjangUserStorage = JSON.parse(localStorage.getItem("keranjangUser"));
      let itemKeranjangUserStorage = keranjangUserStorage.map(itemKeranjangUserStorage => itemKeranjangUserStorage.id);
      // mencocokan data dari idx dengan id yg ada distorage
      let index = itemKeranjangUserStorage.findIndex(id => id == idx);
      // menghapus
      this.keranjangUser.splice(index,1)
      // ini untuk menyimpan kondisi terbaru
      const parsed = JSON.stringify(this.keranjangUser);
        localStorage.setItem('keranjangUser', parsed);
      window.location.reload();
    },

    checkout(){
      let productId = this.keranjangUser.map(function(product) {
        return product.id;
      })

      let checkoutData = {
        'name': this.customerInfo.name,
        'email': this.customerInfo.email,
        'number': this.customerInfo.number,
        'address': this.customerInfo.address,
        'transaction_total': this.totalHarga,
        'transaction_status': "PENDING",
        'transaction_details': productId
      };

      axios
      .post("http://127.0.0.1:8000/api/checkout", checkoutData)
      .then(() => (this.$router.push("success")))
      .catch(err => console.log(err))
    }
  },
  mounted() {
    if (localStorage.getItem('keranjangUser')) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
      } catch(e) {
        localStorage.removeItem('keranjangUser');
      }
    }
  },
  computed: {
    subHarga(){
      return this.keranjangUser.reduce(function(items, data){
        return items + data.price;
      }, 0);
    },
    totalPajak(){
      return (this.subHarga * 10  / 100);
    },
    totalHarga(){
      return (this.subHarga + this.totalPajak);
    }
  },
};
</script>

<style scoped>
.img-cart{
  width: 100px;
  height: 100px;
}
</style>