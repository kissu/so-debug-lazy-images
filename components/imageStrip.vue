<template>
  <div>
    <Observer
      v-if="imageIndexes"
      :indexes="imageIndexes"
      :inView="inView"
      @scrollId="inViews"
    />
    <div class="topRight">
      <h2>CurrentImage:{{ inView + 1 }}</h2>
    </div>
    <div
      class="text-center"
      v-for="(item, index) in convertedImages"
      :key="item.name"
    >
      <div :data-step="index">
        <div v-if="item.name === 'ad'">
          <img :src="item.file" />
        </div>
        <ImageLoader v-if="item.name !== 'ad'">
          <img
            loading="lazy"
            slot="image"
            :src="item.file"
            :alt="`Image #${index}`"
          />
          <div slot="preloader">
            <span>Image is loading! </span>
            <p>{{ index + 1 }}</p>
          </div>
          <span slot="error">
            <div>
              <span>Error Loading Image x.x </span>
              <button>Button for loading Image (Not Working)</button>
            </div>
          </span>
        </ImageLoader>
      </div>
    </div>
  </div>
</template>

<script>
import Observer from "./Observer.vue";
import ImageLoader from "./ImageLoader.vue";
export default {
  data: () => ({
    inView: null,
    imageIndexes: null,
  }),
  props: {
    allImg: {
      type: Array,
      required: true,
    },
  },
  components: { Observer, ImageLoader },
  mounted() {
    this.indexList();
  },
  computed: {
    convertedImages() {
      const firstImg = this.allImg[0];
      const lastImg = this.allImg[this.allImg.length - 1];
      const middle = this.allImg.slice(1, -1);
      const banner = {
        name: "ad",
        file: `https://via.placeholder.com/468x60?text=custom+banner+or+Image"`,
      };
      const banner2 = {
        name: "ad",
        file: `https://via.placeholder.com/468x60?text=custom+banner+or+Image+2"`,
      };
      if (this.allImg.length >= 4) {
        // const half = Math.floor(this.allImg.length/2)
        // let firstPart = this.allImg.slice(0,half)
        // let lastPart = this.allImg.slice(half,)
        // const banner = {name: "ad"}
        // const banner2 = {name: "fullBanner"}
        // return [firstImg,...firstPart, banner, ...lastPart,lastImg]
        return [firstImg, banner, ...middle, banner2, lastImg];
      } else {
        return this.allImg;
      }
    },
  },
  methods: {
    indexList() {
      let arrKey = [];
      for (const key in this.allImg) {
        arrKey.push(key);
      }
      this.imageIndexes = arrKey;
    },
    inViews(value) {
      this.inView = parseFloat(value);
      this.$emit("scrollId", this.inView);
    },
  },
};
</script>

<style scoped>
.topRight {
  position: fixed;
  top: 0px;
  right: 0px;
}
</style>