<template lang="">
  <div class="container mt-5">
    <div class="">
      <h3>前測</h3>
      <p>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Possimus quas
        eligendi non illum ad quos provident mollitia deleniti sequi numquam,
        velit eum magnam doloribus molestiae aut quam in suscipit amet!
      </p>
      <div class="text-center">
        <button @click="next" class="btn btn-primary mt-3">開始</button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {};
  },
  mounted() {},
  methods: {
    async next() {
      this.$router.push({
        path: "/pretest/rating",
        query: { ...this.$route.query },
      });
    },
  },
};
</script>
<style lang=""></style>
