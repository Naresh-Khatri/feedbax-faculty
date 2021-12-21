<template>
  <q-page class="flex flex-center" style='overflow:hidden'>
    <Item
      data-aos="fade-left"
      v-for="(branch, index) in branches"
      :key="index"
      :title="branch.branchName"
    />
  </q-page>
</template>

<script>
import axios from "axios";
import "aos/dist/aos.css";
import AOS from "aos";

import Item from "../components/Item.vue";

import branches from "../data/data.js";

export default {
  name: "PageIndex",
  components: {
    Item,
  },
  data() {
    return {
      branches:branches
    };
  },
  async mounted() {
    console.log(this.branches);
    AOS.init({
      offset: 120, // offset (in px) from the original trigger point
      delay: 0, // values from 0 to 3000, with step 50ms
      duration: 200, // values from 0 to 3000, with step 50ms
      easing: "ease", // default easing for AOS animations
      once: false, // whether animation should happen only once - while scrolling down
      mirror: false, // whether elements should animate out while scrolling past them
      anchorPlacement: "top-bottom", // defines which position of the element regarding to window should trigger the animation
    });
    // try {
    //   const { data } = await axios.get("https://feedbax.plasmatch.in/");
    //   // const { data } = await axios.get("http://localhost:5000/all");
    //   this.branches = data;
    // } catch (e) {
    //   console.log(e);
    // }
  },
};
</script>
