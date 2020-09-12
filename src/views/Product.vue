<template>
  <div class="product">
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
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="img_big" alt />
                </div>
                <div class="product-thumbs" v-if="productDetails.galleries && productDetails.galleries.length > 0">
                  <carousel class="product-thumbs-track ps-slider mx-auto" :nav="false" :dots="false">
                    <div v-for="foto in productDetails.galleries" :key="foto.id" class="pt" @click="changeImage(foto.photo)" :class="foto.photo == img_big ? 'active' : ''">
                      <img :src="foto.photo" alt="" />
                    </div>

                    <!-- <div class="pt" @click="changeImage(thumbs[1])" :class="thumbs[1] == img_big ? 'active' : ''">
                      <img src="img/mickey2.jpg" alt="" />
                    </div>

                    <div class="pt" @click="changeImage(thumbs[2])" :class="thumbs[2] == img_big ? 'active' : ''">
                      <img src="img/mickey3.jpg" alt="" />
                    </div>

                    <div class="pt" @click="changeImage(thumbs[3])" :class="thumbs[3] == img_big ? 'active' : ''">
                      <img src="img/mickey4.jpg" alt="" />
                    </div> -->
                  </carousel>
                </div>
                <div class="col-lg-12" v-else>
                  <p>Produk belum tersedia untuk saat ini.</p>
                </div>
              </div>
              
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{ productDetails.type }}</span>
                    <h3>{{ productDetails.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <p v-html="productDetails.description"></p>
                    <h4>Rp. {{ productDetails.price }}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/cart" class="primary-btn pd-cart">
                      <a href="#" @click="saveKeranjang(productDetails.id, productDetails.name, productDetails.price, productDetails.galleries[0].photo)" class="primary-btn pd-cart">Add To Cart</a>
                    </router-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Product Shop Section End -->

    <RelatedShayna />
    <FooterShayna />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderShayna from "@/components/HeaderShayna.vue";
import FooterShayna from "@/components/FooterShayna.vue";
import RelatedShayna from "@/components/RelatedShayna.vue";
import carousel from "vue-owl-carousel";

import axios from "axios";

export default {
  name: "Product",
  components: {
    HeaderShayna,
    FooterShayna,
    RelatedShayna,
    carousel
  },
  data() {
    return {
      img_big: "",
      productDetails:[],
      keranjangUser: []
    };
  },
  methods: {
      changeImage(urlImage){
          this.img_big = urlImage;
      },
      setDataPhoto(data){
        // ini mengisi obejct productDetails dengan data dari API
        this.productDetails = data;
        // ini untuk mengisi img_big dengan data dari APi(galleries)
        this.img_big = data.galleries[0].photo;
      },
      saveKeranjang(idProduct, nameProduct,priceProduct,imgProduct){
        const productStorage = {
          "id": idProduct,
          "photo": imgProduct,
          "name": nameProduct,
          "price": priceProduct
        }

        this.keranjangUser.push(productStorage);
        const parsed = JSON.stringify(this.keranjangUser);
        localStorage.setItem('keranjangUser', parsed);
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
    axios
    .get("http://127.0.0.1:8000/api/products", {
      params: {
        id: this.$route.params.id
      }
    })
    .then(res => (this.setDataPhoto(res.data.data)))
    .catch(err => console.log(err))
  },
};
</script>


<style scoped>
.pt {
  margin-right: 12px;
}
.pd-cart{
  padding: -20px;
}
</style>