<template lang="">
  <div class="container mt-5">
    <div class="">
      <h3>前測</h3>
      <p>在此請您紀錄您對此聊天對象的名字與頭像的感覺</p>
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
        alert("您已經完成前測，請直接關閉視窗回到聊天機器人繼續實驗");
        return;
      }
      this.$router.push({
        path: "/pretest/rating",
        query: { ...this.$route.query },
      });
    },
  },
};
</script>
<style lang=""></style>
