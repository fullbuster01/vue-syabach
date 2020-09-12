<template>
  <div class="banner-shayna">
    <!-- Women Banner Section Begin -->
    <section class="women-banner spad">
      <div class="container-fluid">
        <div class="row">
          <div class="col-lg-12 mt-5" v-if="products && products.length > 0">
            <carousel
              class="product-slider"
              :items="3"
              :nav="false"
              :autoplay="true"
              :dots="false"
              :loop="true"
            >
              <div class="product-item" v-for="itemProduct in products" :key="itemProduct.id">
                <div class="pi-pic">
                  <img :src="itemProduct.galleries[0].photo" alt />
                  <ul>
                    <li class="w-icon active">
                      <!-- <router-link :to="'/'"> -->
                      <a
                        href="#"
                        @click="saveKeranjang(itemProduct.id, itemProduct.name, itemProduct.price, itemProduct.galleries[0].photo)"
                      >
                        <i class="icon_bag_alt"></i>
                      </a>
                      <!-- </router-link> -->
                    </li>
                    <li class="quick-view">
                      <router-link :to="'/product/'+itemProduct.id" class="f">+ Quick View</router-link>
                    </li>
                  </ul>
                </div>
                <div class="pi-text">
                  <div class="catagory-name">{{itemProduct.type}}</div>
                  <router-link :to="'/product/'+itemProduct.id">
                    <h5>{{itemProduct.name}}</h5>
                  </router-link>
                  <div class="product-price">
                    Rp. {{itemProduct.price}}
                    <!-- <span>$35.00</span> -->
                  </div>
                </div>
              </div>
              <!-- <div class="product-item">
                <div class="pi-pic">
                  <img src="img/products/women-2.jpg" alt />
                  <ul>
                    <li class="w-icon active">
                      <a href="#">
                        <i class="icon_bag_alt"></i>
                      </a>
                    </li>
                    <li class="quick-view">
                      <a href="#">+ Quick View</a>
                    </li>
                  </ul>
                </div>
                <div class="pi-text">
                  <div class="catagory-name">Shoes</div>
                  <a href="#">
                    <h5>Guangzhou sweater</h5>
                  </a>
                  <div class="product-price">$13.00</div>
                </div>
              </div>
              <div class="product-item">
                <div class="pi-pic">
                  <img src="img/products/women-3.jpg" alt />
                  <ul>
                    <li class="w-icon active">
                      <a href="#">
                        <i class="icon_bag_alt"></i>
                      </a>
                    </li>
                    <li class="quick-view">
                      <a href="#">+ Quick View</a>
                    </li>
                  </ul>
                </div>
                <div class="pi-text">
                  <div class="catagory-name">Towel</div>
                  <a href="#">
                    <h5>Pure Pineapple</h5>
                  </a>
                  <div class="product-price">$34.00</div>
                </div>
              </div>
              <div class="product-item">
                <div class="pi-pic">
                  <img src="img/products/women-4.jpg" alt />
                  <ul>
                    <li class="w-icon active">
                      <a href="#">
                        <i class="icon_bag_alt"></i>
                      </a>
                    </li>
                    <li class="quick-view">
                      <a href="#">+ Quick View</a>
                    </li>
                    <li class="w-icon">
                      <a href="#">
                        <i class="fa fa-random"></i>
                      </a>
                    </li>
                  </ul>
                </div>
                <div class="pi-text">
                  <div class="catagory-name">Towel</div>
                  <a href="#">
                    <h5>Converse Shoes</h5>
                  </a>
                  <div class="product-price">$34.00</div>
                </div>
              </div>-->
            </carousel>
          </div>

          <div class="col-lg-12" v-else>
            <p>Produk belum tersedia untuk saat ini.</p>
          </div>
        </div>
      </div>
    </section>
    <!-- Women Banner Section End -->
  </div>
</template>


<script>
import carousel from "vue-owl-carousel";
import axios from "axios";
export default {
  name: "BannerShayna",
  components: {
    carousel,
  },
  data() {
    return {
      products: [],
      keranjangUser: [],
    };
  },
  mounted() {
    if (localStorage.getItem("keranjangUser")) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));
      } catch (e) {
        localStorage.removeItem("keranjangUser");
      }
    }
    axios
      .get("http://127.0.0.1:8000/api/products")
      .then((res) => (this.products = res.data.data.data))
      .catch((err) => console.log(err));
  },
  methods: {
    saveKeranjang(idProduct, nameProduct, priceProduct, imgProduct) {
      const productStorage = {
        id: idProduct,
        photo: imgProduct,
        name: nameProduct,
        price: priceProduct,
      };

      this.keranjangUser.push(productStorage);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);

      // ini untuk mereplace halaman
      window.location.reload();
    },
  },
};
</script>

<style scoped>
.product-item {
  margin-right: 20px;
}
@media only screen and (max-width: 767px) {
  router-link {
    font-size: 2px;
  }
}
</style>