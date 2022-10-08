<template lang="">
  <div class="container mt-5">
    <div class="">
      <h3>聊天體驗測驗</h3>
      <p>
        在此階段我們將使用可量化的量表來評估您的聊天體驗，請您記得使用「查看聊天記錄」功能回顧聊天過程並對聊天對象給予評價。
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
        path: "/posttest/ueq_cuq",
        query: { ...this.$route.query },
      });
    },
  },
};
</script>
<style lang=""></style>
