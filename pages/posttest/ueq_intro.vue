<template lang="">
  <div class="container mt-5">
    <div class="">
      <h3>聊天體驗評估</h3>
      <p>
        在此階段我們將使用可量化的量表來評估您的聊天體驗，請您記得使用「查看聊天記錄」功能回顧聊天過程並對聊天對象給予評價。
      </p>
      <p>
        敬請您耐心完成所有問題，我們會使用比較多的問卷題目，是因為要跑一個比較複雜的統計分析來找出適合的聊天機器人設計，非常感謝您提供的寶貴實驗資料。
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
