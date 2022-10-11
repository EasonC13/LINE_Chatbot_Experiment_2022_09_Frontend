<template lang="">
  <div class="">
    <div class="mx-2 mt-3">
      <h3>人格測驗</h3>
      <p>
        請選擇對下方人格特質的是否適用於您，舉例來說，如果題目是「喜歡與人相處」，請選擇您是否同意「自己是一個喜歡與人相處的人」。
      </p>
      <p>分數代表意義如下：</p>
      <ol>
        <li>非常不同意</li>
        <li>稍微不同意</li>
        <li>普通（中立）</li>
        <li>稍微同意</li>
        <li>非常同意</li>
      </ol>
    </div>

    <div
      class="my-3 mx-2"
      v-for="(question, q_num) in big5"
      :key="`big5_${q_num}`"
    >
      <p class="h5">{{ q_num + 1 }}. {{ question }}</p>
      <div class="clearfix mb-0">
        <span class="float-left text-secondary">非常不同意</span>
        <span class="float-right text-secondary">非常同意</span>
        <div class="text-center"></div>
      </div>

      <div class="text-center">
        <button
          class="btn"
          type="button"
          :class="{
            'btn-primary': selected[q_num] == index + 1,
            'btn-outline-dark': !(selected[q_num] == index + 1),
            'px-0': true,
          }"
          style="width: 12vw"
          @click="select(q_num, num)"
          v-for="(num, index) in range"
          :key="`${index}`"
        >
          {{ num }}
        </button>
      </div>
    </div>
    <div class="text-center mb-2">
      <button @click="submit" class="btn btn-primary" :disabled="!form_finish">
        已完成，進入下一個測驗
      </button>
    </div>
  </div>
</template>
<script>
//  使用
import VueForm from "@lljj/vue-json-schema-form";
import big5 from "./big5";
export default {
  name: "Demo",
  emits: ["submit"],
  components: {
    VueForm,
  },
  watch: {},
  data() {
    return {
      loading: false,
      current_index: 0,
      bots: [],
      selected: Array(big5.length).fill(0),
      range: [1, 2, 3, 4, 5],
      big5: big5,
      submitted: false,
      lock: false,
    };
  },
  computed: {
    form_finish() {
      let res = this.selected.filter((x) => x == 0);
      return res.length == 0;
    },
  },
  methods: {
    select(q_num, i) {
      this.selected[q_num] = i;
      this.selected = [...this.selected];
    },
    async submit() {
      if (this.lock) {
        return 0;
      } else {
        this.lock = true;
        let res = await this.$axios.$post("/api/v1/starter/big5", {
          userId: this.$route.query.id,
          big5: JSON.stringify(this.selected),
        });
        if (res.acknowledged) {
          this.next();
        } else {
          alert("發生未知錯誤，請稍後重試或聯絡管理員");
        }
        this.lock = false;
      }
    },
    next() {
      this.$router.push({
        path: "/starter/attachment",
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
