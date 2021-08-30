<template>
  <div class="observer" />
</template>

<script>
export default {
  props: ["indexes", "inView"],
  data: () => ({
    observer: null,
    activeBlock: null,
    blockEls: [],
    blockBreakpoints: [],
  }),
  created() {
    this.activeBlock = this.inView;
  },
  mounted() {
    // if (process.client) {
    this.initObserver();
    // }
  },
  beforeDestroy() {
    this.observer.disconnect();
  },
  methods: {
    initObserver() {
      this.blockEls = this.indexes.map((el) =>
        document.querySelector(`[data-step = '${el}']`)
      );
      const options = {
        threshold: 0.35,
      };
      const observer = new IntersectionObserver(
        this.handleIntersection,
        options
      );
      this.blockEls.forEach((el) => observer.observe(el));
      this.observer = observer;
    },
    handleIntersection(entries) {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          const targetValue = entries[0].target.attributes[1].value;
          this.activeBlock = Number(targetValue);
        }
      });
    },
  },
  watch: {
    activeBlock() {
      this.$emit("scrollId", this.activeBlock);
    },
  },
};
</script>