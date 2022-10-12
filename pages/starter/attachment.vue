<template lang="">
  <div class="mx-2 mt-3">
    <h3>親密關係人個特質單選</h3>
    <p>請在以下敘述中，選出最能夠代表您在親密關係中的個人經驗。</p>
    <div
      class="card mx-2"
      :class="{ selected: selected == index + 1 }"
      style="width: 18rem"
      @click="select(index + 1)"
      v-for="(question, index) in questions"
      :key="index"
    >
      <div class="card-body">
        <p class="card-text">
          {{ question }}
        </p>
      </div>
    </div>

    <div class="text-center my-2">
      <button @click="submit" class="btn btn-primary" :disabled="selected == 0">
        已完成，進入下一個測驗
      </button>
    </div>
  </div>
</template>
<script>
//  使用
import VueForm from "@lljj/vue-json-schema-form";

export default {
  name: "Demo",
  emits: ["submit"],
  components: {
    VueForm,
  },
  watch: {},
  data() {
    return {
      selected: 0,
      questions: [
        "與對方親近時我會覺得有些不自在；我也發現自己很難信任以及依賴對方。當對方太親近我時，我會覺得緊張，而且，比起我感受到的舒服自在，對方常常希望我能夠更加的親密",
        "我發現與對方親近是容易的，而且也覺得與對方互相依賴是舒服的。我也並不需要擔心被拋棄或有人與我親近。",
        "我總覺得對方不願意表現出我想要的親近。我常常擔心我的伴侶並不是真正的愛我或是不想要和我在一起。我時候會因為想要和伴侶保持很親密的關係，而嚇到對方。",
      ],
    };
  },
  mounted() {
    console.log("this.$route", this.$route);
  },
  methods: {
    select(i) {
      if (i == this.selected) {
        this.selected = 0;
      } else {
        this.selected = i;
      }
    },
    async submit() {
      console.log(this.selected);
      if (this.selected == 0) {
        alert("請選擇一個最適合的描述後再點選繼續");
      } else {
        let res = await this.$axios.$post("/api/v1/starter/attachment-style", {
          userId: this.$route.query.id,
          style: this.selected,
        });
        if (res.acknowledged) {
          this.next();
        } else {
          alert("發生未知錯誤，請稍後重試或聯絡管理員");
        }
      }
    },
    next() {
      this.$router.push({
        path: "/starter/final",
        query: { ...this.$route.query },
      });
    },
  },
};
</script>
<style scoped>
.selected {
  border-color: blue;
  color: blue;
  /* box-shadow: 0 10px 20px rgba(0, 0, 0, 0.12), 0 4px 8px rgba(0, 0, 0, 0.06); */
}
</style>
