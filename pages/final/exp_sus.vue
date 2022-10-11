<template lang="">
  <div class="">
    <h3 class="mx-2 mt-3">對於實驗過程的使用體驗調查表</h3>
    <p class="mx-2 text-secondary">此為本實驗最後一份問卷</p>
    <p class="mt-1 mx-2 h6">對於下列敘述，請問您的同意程度？</p>
    <div
      class="my-3 mx-2"
      v-for="(question, q_num) in sus"
      :key="`cuq_${q_num}`"
    >
      <p class="mb-1">{{ q_num + 1 }}. {{ question }}</p>
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
          :key="`sus_${index}`"
        >
          {{ num }}
        </button>
      </div>
    </div>

    <div class="mx-2">
      <p class="mb-0 mt-3">對本次實驗有任何建議嗎？</p>
      <div class="input-group">
        <textarea
          v-model="suggestion"
          class="form-control"
          aria-label="With textarea"
          placeholder="請輸入您的建議"
          style="height: 50vh"
        ></textarea>
      </div>
    </div>

    <div class="text-center my-2">
      <button @click="submit" class="btn btn-primary" :disabled="!form_finish">
        下一步
      </button>
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
      selected: Array(10).fill(0),
      range: [1, 2, 3, 4, 5],
      sus: [
        "我想我會願意再次參與類似實驗。",
        "我覺得本次實驗過於複雜。",
        "我認為本次實驗很容易使用。",
        "我想我需要有人幫助才能使用本次實驗。",
        "我覺得本次實驗的功能整合得很好。",
        "我覺得本次實驗有太多不一致的地方。",
        "我可以想像大部份的人很快就可以學會使用本次實驗。",
        "我覺得本次實驗使用起來很麻煩。",
        "我很有自信能使用本次實驗。",
        "我需要學會很多額外的資訊，才能使用本次實驗。",
      ],
      submitted: false,
      lock: false,
      suggestion: "",
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
      await this.$axios.$post("/api/v1/final/exp_sus", {
        userId: this.$route.query.id,
        sus: JSON.stringify(this.selected),
        suggestion: this.suggestion,
      });
      this.next();
    },
    next() {
      this.$router.push({
        path: "/final/final",
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
