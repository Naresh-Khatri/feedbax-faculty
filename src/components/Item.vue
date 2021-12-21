<template>
  <router-link :to="'/feedback/?id=' + profile_id">
    <div class="item" :class="completed ? 'inactive' : ''">
      <!-- <q-icon
        style="left: 0rem; top:-5em"
        name="task_alt"
        size="md"
      ></q-icon> -->
      <div class="item-content">
        <div class="text-h6 item-title">{{ title }}</div>
        <div class="text-caption item-description">{{ name }}</div>
      </div>
      <div class="item-image">
        <img data-aos='fade-up' src="https://avataaars.io/" ref="profileImg" width="150" />
        <!-- :src="imgSrc"
          alt="profile avataaaaar" -->
      </div>
    </div>
  </router-link>
</template>

<script>
import AOS from "aos";
import "aos/dist/aos.css";
import fastAverageColor from "fast-average-color";
export default {
  props: ["profile_id", "title", "name"],
  data() {
    return {
      avgBackgroundColor: "#2b0047",
      imgSrc: `https://avataaars.io/`,
    };
  },
  mounted() {
    AOS.init({
      offset: 120, // offset (in px) from the original trigger point
      delay: 100, // values from 0 to 3000, with step 50ms
      duration: 400, // values from 0 to 3000, with step 50ms
      easing: "ease-in-out", // default easing for AOS animations
      once: false, // whether animation should happen only once - while scrolling down
      mirror: false, // whether elements should animate out while scrolling past them
      anchorPlacement: "top-bottom", // defines which position of the element regarding to window should trigger the animation
    });
    // let img = new Image();
    // img.src = this.imgSrc;
    // // let canvas = document.createElement("canvas");
    // this.$refs.profileImg.width = img.width;
    // this.$refs.profileImg.height = img.height;
    // this.$refs.profileImg
    //   .getContext("2d")
    //   .drawImage(img, 0, 0, img.width, img.height);
    // setTimeout(() => {
    //   let context = this.$refs.profileImg.getContext("2d");
    //   let imgd = context.getImageData(0, 0, 150, 150);
    //   let pix = imgd.data;
    //   let avg = []
    //   // Loop over each pixel and invert the color.
    //   // for (let i = 0, n = pix.length; i < n; i += 4) {
    //   //   avg[i]  =avg[i] + pix[i]
    //   //   pix[i + 0] = 255 - pix[i]; // red
    //   //   pix[i + 1] = 255 - pix[i + 1]; // green
    //   //   pix[i + 2] = 255 - pix[i + 2]; // blue
    //     // i+3 is alpha (the fourth element)
    //   // }
    //   // console.log(pix);

    //   // const fac = new fastAverageColor();
    //   // fac
    //   //   .getColorAsync(this.$refs.profileImg)
    //   //   .then((color) => {
    //   //     this.avgBackgroundColor = color.rgb;
    //   //     console.log(this.avgBackgroundColor);
    //   //   })
    //   //   .catch((err) => {
    //   //     console.log(err);
    //   //   });
    // }, 1000);
  },
  computed: {
    completed() {
      return localStorage.getItem("completed_ids")
        ? localStorage.getItem("completed_ids").includes(this.profile_id)
        : false;
    },
  },
};
</script>

<style>
.item {
  width: 330px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  padding-left: 30px;
  margin: 20px;
  color: white;
  border-radius: 20px;
  background: linear-gradient(to right, #222222, #2b0047);
  transition: 0.3s;
}
/* .item:hover{
  background: linear-gradient(to right, #222222, #222222);
transition: 0.3s;
} */
.inactive {
  background: linear-gradient(to right, #222222, #222222);
}
a {
  text-decoration: none;
}
</style>
