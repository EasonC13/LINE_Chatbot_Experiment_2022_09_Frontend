<template lang="">
  <div class="container mt-5">
    <div class="">
      <h3>End</h3>
      <p>
        您已選擇終止實驗流程，請直接關閉本視窗。
        如果是誤觸或者您回心轉意，請點選「返回」或於 LINE
        聊天機器人重新點選「繼續」
      </p>
      <div class="text-center">
        <button @click="next" class="btn btn-primary">關閉視窗</button>
        <button @click="back" class="btn btn-primary">返回</button>
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
    next() {
      window.location = "https://line.me/R/oaMessage/@956unykg/?";
    },
    back() {
      this.$router.push({
        path: "/starter/",
        query: { ...this.$route.query },
      });
    },
  },
};
</script>
<style lang=""></style>
