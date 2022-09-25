<template lang="">
  <div class="container mt-5">
    <div class="">
      <h3>End</h3>
      <p>已完成全部實驗，匯款將於一週後撥款並通過 Email 通知</p>
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
      window.location = "https://line.me/R/oaMessage/@956unykg/?";
    },
  },
};
</script>
<style lang=""></style>
