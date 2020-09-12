<template>
  <div>
    <!-- Partner Logo Section Begin -->
    <div class="partner-logo">
      <div class="container">
        <carousel class="logo-carousel" :nav="false" :items="4" :dots="false">
          <div class="logo-item">
            <div class="tablecell-inner">
              <a href="#">
              <img src="img/logo-market/5.png" alt />
              </a>
            </div>
          </div>
          <div class="logo-item">
            <div class="tablecell-inner">
              <a href="#">
              <img src="img/logo-market/2.png" alt />
              </a>
            </div>
          </div>
          <div class="logo-item">
            <div class="tablecell-inner">
              <a href="#">
              <img src="img/logo-market/3.png" alt />
              </a>
            </div>
          </div>
          <div class="logo-item">
            <div class="tablecell-inner">
              <a href="#">
              <img src="img/logo-market/4.png" alt />
              </a>
            </div>
          </div>
          <!-- <div class="logo-item">
            <div class="tablecell-inner">
              <img src="img/logo-carousel/logo-5.png" alt />
            </div>
          </div> -->
        </carousel>
      </div>
    </div>
    <!-- Partner Logo Section End -->
  </div>
</template>

<script>
import carousel from 'vue-owl-carousel'
export default {
  name: "PartnerShayna",
   components: { carousel }
};
</script>