<template>
  <div>
    <div><h1>Hello</h1></div>
    <ImageStrip :allImg="imageArray" />
  </div>
</template>

<script>
import ImageStrip from "@/components/imageStrip.vue";
export default {
  data: () => ({
    imageArray: [],
    noOfImg: 10,
  }),
  components: { ImageStrip },
  mounted() {
    this.setImageArr();
  },
  methods: {
    setImageArr() {
      for (var i = 0; i < this.noOfImg; i++) {
        this.imageArray.push({
          name: i,
          file: `https://via.placeholder.com/200`,
        });
      }
    },
  },
};
</script>