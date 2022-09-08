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
        <button @click="next" class="btn btn-primary">
          已閱讀完說明，開始實驗前測試
        </button>
      </div>
    </div>
    <!-- <div class="" v-if="status == 'big5'">
      <starterBig5 @submit="start_attachment"></starterBig5>
    </div> -->
    <!-- <div class="" v-if="status == 'attachment_style'">
      <h3>attachment_style</h3>
      <p>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Possimus quas
        eligendi non illum ad quos provident mollitia deleniti sequi numquam,
        velit eum magnam doloribus molestiae aut quam in suscipit amet!
      </p>
      <div class="text-center">
        <button @click="start_end" class="btn btn-primary">
          已完成，進入結尾
        </button>
      </div>
    </div>
    <div class="" v-if="status == 'end'">
      <h3>End</h3>
      <p>已完成本階段所有流程，請回到聊天機器人並點選「下一步」繼續實驗</p>
      <div class="text-center">
        <button @click="close" class="btn btn-primary">關閉視窗</button>
      </div>
    </div> -->
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
      this.$router.push({
        path: "/starter/big5",
        query: { ...this.$route.query },
      });
    },
  },
};
</script>
<style lang=""></style>
