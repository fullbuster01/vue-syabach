<template>
  <div class="insta-shayna">
    <!-- Instagram Section Begin -->
    <div class="instagram-photo row">
      <div
        class="insta-item set-bg col-md-3"
        v-for="ig in instagram.slice(0, 8)"
        :key="ig.id"
        :style="{ backgroundImage: `url(${ig.media_url})` }"
      >
        <div class="inside-text">
          <a :href="ig.permalink">
            <i class="ti-instagram"></i>
          </a>
          <h5>
            <a :href="ig.permalink">{{ ig.username }}</a>
          </h5>
        </div>
      </div>
      <!-- <div class="insta-item set-bg" style="background-image: url('img/insta-2.jpg');">
        <div class="inside-text">
          <i class="ti-instagram"></i>
          <h5>
            <a href="#">shayna_gallery</a>
          </h5>
        </div>
      </div>
      <div class="insta-item set-bg" style="background-image: url('img/insta-3.jpg');">
        <div class="inside-text">
          <i class="ti-instagram"></i>
          <h5>
            <a href="#">shayna_gallery</a>
          </h5>
        </div>
      </div>
      <div class="insta-item set-bg" style="background-image: url('img/insta-4.jpg');">
        <div class="inside-text">
          <i class="ti-instagram"></i>
          <h5>
            <a href="#">shayna_gallery</a>
          </h5>
        </div>
      </div>
      <div class="insta-item set-bg" style="background-image: url('img/insta-5.jpg');">
        <div class="inside-text">
          <i class="ti-instagram"></i>
          <h5>
            <a href="#">shayna_gallery</a>
          </h5>
        </div>
      </div>
      <div class="insta-item set-bg" style="background-image: url('img/insta-6.jpg');">
        <div class="inside-text">
          <i class="ti-instagram"></i>
          <h5>
            <a href="#">shayna_gallery</a>
          </h5>
        </div>
      </div>-->
    </div>
    <!-- Instagram Section End -->
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "InstaShayna",

  data() {
    return {
      instagram: []
    };
  },
  mounted() {
    axios
      .get(
        "https://graph.instagram.com/me/media?access_token=IGQVJWcDZAWYmxiM3BHSlQ0bVBYVFFVajRpOXlhQnlUN25iLWZApa2tOLTdOSkJRMHdVSHE1bnVpam5FTHNUaTR2VjRWRzRkU0M5cVlPaWRJV0gtcmFFeVVhZAG1BYWtiMDdaYWdidnUwWUtfVEFwazBySQZDZD&fields=id,username,media_type,media_url,permalink"
      )
      .then(res => (this.instagram = res.data.data))
      .catch(err => console.log(err));
  }
};
</script>