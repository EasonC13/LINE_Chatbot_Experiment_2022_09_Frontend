<template lang="">
  <div class="container mt-5">
    <div class="">
      <h3>最終資料收集</h3>
      <p>
        在此階段我們會收取您對本實驗的心得，並且向您詢問基本個人資料及銀行帳號，用於發送實驗報酬。
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
      let res = await this.$axios.$get(
        `/api/v1/general/isfinish?userId=${this.$route.query.id}&condition=${this.$route.query.test}`
      );
      if (res.isfinish) {
        alert("您已經完成最終測驗，請直接關閉視窗回到聊天機器人繼續實驗");
        return;
      }
      this.$router.push({
        path: "/final/tam",
        query: { ...this.$route.query },
      });
    },
  },
};
</script>
<style lang=""></style>
