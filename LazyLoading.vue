<template lang="pug">
.lazy
  .lazy-loading(v-if="!lazyToggle")
    p loading ...
  .lazy-content(v-else)
    slot
</template>

<script>
export default {
  name: "LazyLoading",
  props: {
    lazyToggle: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    randomClass() {
      return Math.floor(Math.random());
    }
  },
  mounted() {
    let lazySection = document.querySelector(".lazy");
    let lazyIntersection = new IntersectionObserver(
      entries => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            this.$emit("lazyChange", { value: this.lazyToggle });
            this.lazyToggle = true;
          }
        });
      },
      { rootMargin: "0px 0px 100px 0px" }
    );
    lazyIntersection.observe(lazySection);
  }
};
</script>
<style lang="scss">
.lazy {
  &-loading {
    height: 10rem;
    background-color: rgba(0, 0, 0, 0.3);
  }
}
</style>
