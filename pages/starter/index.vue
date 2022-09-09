<template lang="">
  <div class="container mt-5">
    <div class="">
      <h3>實驗說明</h3>
      <p>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Possimus quas
        eligendi non illum ad quos provident mollitia deleniti sequi numquam,
        velit eum magnam doloribus molestiae aut quam in suscipit amet!
      </p>
      <div class="text-center">
        <button @click="next" class="btn btn-primary mt-3">
          已閱讀並同意說明，開始實驗
        </button>
        <button @click="deny" class="btn btn-secondary mt-3">
          不同意參與實驗，停止並退出
        </button>
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
    deny() {
      this.$router.push({
        path: "/starter/deny",
        query: { ...this.$route.query },
      });
    },
    async next() {
      await this.$axios.$post("/api/v1/starter/accept-term-of-test", {
        userId: this.$route.query.id,
        accept: true,
      });
      this.$router.push({
        path: "/starter/big5",
        query: { ...this.$route.query },
      });
    },
  },
};
</script>
<style lang=""></style>
