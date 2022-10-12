<template lang="">
  <div class="container mt-3">
    <h1 id="-">事後解釋</h1>
    <p>
      本實驗的核心為探討「不同個性的人對於不同的聊天機器人設計，會不會有不同的感受」，因此於實驗一開始時請您填寫的人格測驗為「大五(Big5)人格測驗量表」，其將人的個性分為五種向度：
    </p>
    <ul>
      <li>經驗的開放性（創造性/好奇 或 一致/謹慎）</li>
      <li>盡責性（高效/有組織 或 奢侈/粗心）</li>
      <li>外向性（外向/精力充沛 或 孤獨/內向）</li>
      <li>親和性（友好/富有同情心 或 批判/理性）</li>
      <li>神經質（敏感/緊張 或 彈性/自信）</li>
    </ul>
    <p>
      而在剛剛聊天過程中，您遇到的聊天 AI
      對象，也都是基於此五種個性去設計的，即他的個性可能是外向、內向或冷靜、神經質等十種。並且搭配兩種性別，總共有二十種組合（5x2x2=20），構成您在這四輪聊天的聊天對象。
    </p>
    <p>
      在後續我們會將您的實驗資料去識別化後，分析您的發言、回應對象、聊天時間、量表填寫結果等資料。
    </p>

    <p>請問您對本實驗的後續有興趣嗎？想不想收到後續消息？</p>
    <div @click.prevent="check">
      <input type="checkbox" v-model="checked"></input>我想收到後續消息，請把後續的實驗、發表、論文資訊寄給我（不會發廣告）
    </div>
    <div class="mx-2">
      <p class="mb-0 mt-3 mb-1">看完事後解釋後，您對本次實驗有任何想法或問題嗎？（回應將統一由後續消息於電子郵件發出，記得勾選上方才能收到）</p>
      <div class="input-group">
        <textarea
          v-model="questions"
          class="form-control"
          aria-label="With textarea"
          placeholder="請輸入您的問題或想法"
          style="height: 50vh"
        ></textarea>
      </div>
    </div>

    <div class="text-center my-2">
      <button @click="submit" class="btn btn-primary">結束實驗</button>
    </div>
  </div>
</template>
<script>
//  使用
import { init } from "events";
export default {
  name: "Demo",
  emits: ["submit"],
  watch: {},
  data() {
    return {
      checked: false,
      questions: "",
    };
  },
  computed: {
    form_finish() {
      let res = this.selected.filter((x) => x == 0);
      return res.length == 0;
    },
  },
  watch:{
    checked(){
      console.log(this.checked);
    }
  },
  methods: {
    check(){
      this.checked = !this.checked
    },
    async submit() {
      await this.$axios.$post("/api/v1/final/followup", {
        userId: this.$route.query.id,
        checked: this.checked,
        questions: this.questions
      });
      this.next();
    },
    next() {
      this.$router.push({
        path: "/final/finish",
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
img {
  position: relative;
  width: 3em;
  height: 3em;
  overflow: hidden;
  border-radius: 50%;
}
.circular--portrait img {
  width: 100%;
  height: auto;
}
</style>
