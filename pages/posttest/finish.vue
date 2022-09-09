<template lang="">
  <div class="container mt-5">
    <div class="">
      <h3>End</h3>
      <p>
        已完成本階段所有流程，請在關閉視窗後回到聊天機器人並點選「下一步」繼續實驗
      </p>
      <div class="text-center">
        <button @click="next" class="btn btn-primary">關閉視窗</button>
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
      window.location = "https://line.me/R/oaMessage/@956unykg/?";();

    },
  },
};
</script>
<style lang=""></style>
