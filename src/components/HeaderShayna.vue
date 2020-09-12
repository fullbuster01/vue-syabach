<template>
  <div class="header-shayna">
    <!-- Header Section Begin -->
    <header class="header-section">
      <div class="header-top">
        <div class="container">
          <div class="ht-left">
            <div class="mail-service">
              <i class="fa fa-envelope"></i> hello.syabach@gmail.com
            </div>
            <div class="phone-service">
              <i class="fa fa-phone"></i> +6281 4444 8888
            </div>
          </div>
        </div>
      </div>
      <div class="container">
        <div class="inner-header">
          <div class="row">
            <div class="col-lg-2 col-md-2">
              <div class="logo">
                <router-link to="/">
                  <img src="img/logo_website.jpg" alt />
                </router-link>
              </div>
            </div>
            <div class="col-lg-7 col-md-5"></div>
            <div class="col-lg-3 text-right col-md-5">
              <ul class="nav-right">
                <li class="cart-icon">
                  <span class="ranjang">Keranjang Belanja &nbsp;</span>
                  <a href="#">
                    <i class="icon_bag_alt"></i>
                    <span>{{ keranjangUser.length }}</span>
                  </a>
                  <div class="cart-hover">
                    <div class="select-items">
                      <table>
                        <tbody v-if="keranjangUser && keranjangUser.length > 0">
                          <tr v-for="keranjang in keranjangUser" :key="keranjang.id">
                            <td class="si-pic">
                              <img class="photo-cart" :src="keranjang.photo" alt />
                            </td>
                            <td class="si-text">
                              <div class="product-selected">
                                <p>Rp. {{ keranjang.price }}</p>
                                <h6>{{ keranjang.name }}</h6>
                              </div>
                            </td>
                            <td @click="removeItem(keranjang.id)" class="si-close">
                              <i class="ti-close"></i>
                            </td>
                          </tr>
                          <!-- <tr>
                            <td class="si-pic">
                              <img src="img/select-product-2.jpg" alt />
                            </td>
                            <td class="si-text">
                              <div class="product-selected">
                                <p>$60.00 x 1</p>
                                <h6>Kabino Bedside Table</h6>
                              </div>
                            </td>
                            <td class="si-close">
                              <i class="ti-close"></i>
                            </td>
                          </tr> -->
                        </tbody>
                        <tbody v-else>
                          <tr>
                            <td>Keranjang Kosong.</td>
                          </tr>
                        </tbody>
                      </table>
                    </div>
                    <div class="select-total">
                      <span>total:</span>
                      <h5>Rp. {{ totalHarga }}</h5>
                    </div>
                    <div class="select-button">
                      <router-link to="/cart" class="view-card">
                        <a href="/cart" class="primary-btn view-card">VIEW CARD</a>
                      </router-link>
                      <!-- <a href="#" class="primary-btn view-card"><router-link to="/cart" style="color:#fff;">VIEW CARD</router-link></a> -->
                      <a href="#" class="primary-btn checkout-btn">CHECK OUT</a>
                    </div>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </header>
    <!-- Header End -->
  </div>
</template>

<script>
export default {
  name: "HeaderShayna",
  data() {
    return {
      keranjangUser: []
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
      // window.location.reload();
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
    totalHarga(){
      return this.keranjangUser.reduce(function(items, data){
        return items + data.price;
      }, 0);
    }
  },
}
</script>

<style scoped>
.photo-cart {
  width: 80px;
  height: 80px;
}
</style>