<template lang="">
  <div class="">
    <h3 class="mx-2">陪聊 AI 的接受度調查表</h3>

    <p class="mt-1 mx-2 h6">對於下列敘述，請問您的同意程度？</p>
    <div
      class="my-3 mx-2"
      v-for="(question, q_num) in questions"
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
      <p class="mb-0 mt-3">
        請問除排解孤獨、娛樂等，您認為 AI 陪伴聊天還能達到什麼目的？
      </p>
      <p>（非必填，字數不限，不過有任何建議我們會非常感激）</p>
      <div class="input-group">
        <textarea
          v-model="textarea"
          class="form-control"
          aria-label="With textarea"
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
import VueForm from "@lljj/vue-json-schema-form";
import { init } from "events";
export default {
  name: "Demo",
  emits: ["submit"],
  components: {
    VueForm,
  },
  watch: {},
  data() {
    return {
      selected: Array(26).fill(1),
      range: [1, 2, 3, 4, 5, 6, 7],
      questions: [
        "我有使用 AI 陪伴聊天的意願。",
        "我很樂意繼續使用 AI 陪我聊天。",
        "如果可以，未來的一個月內，我將會使用 AI 陪我聊天。",
        "使用 AI 陪伴聊天，可以讓我比以前更容易排解孤獨。",
        "使用 AI 陪伴聊天，可以讓我比以前更容易獲得社交。",
        "使用 AI 陪伴聊天，可以讓我比以前更容易紓解壓力。",
        "使用 AI 陪伴聊天，可以讓我比以前更容易獲得娛樂。",
        "使用 AI 陪伴聊天，可以讓我獲得更完整的陪伴相關服務。",
        "使用 AI 陪伴聊天，可以讓我獲得更完整的社交相關服務。",
        "使用 AI 陪伴聊天，可以讓我獲得更完整的紓解壓力相關服務。",
        "使用 AI 陪伴聊天，可以讓我獲得更完整的娛樂相關服務。",
        "使用 AI 陪伴聊天，可以使我更快獲得陪伴。",
        "使用 AI 陪伴聊天，可以使我更快獲得社交。",
        "使用 AI 陪伴聊天，可以使我更快紓解壓力。",
        "使用 AI 陪伴聊天，可以使我更快獲得娛樂。",
        "AI 陪伴聊天有助於我排解孤獨。",
        "AI 陪伴聊天有助於我社交。",
        "AI 陪伴聊天有助於我紓解壓力。",
        "AI 陪伴聊天有助於我娛樂。",
        "我覺得 AI 陪伴聊天 的介面是清楚且容易理解。",
        "我操作 AI 陪伴聊天 不用花很多精神心力。",
        "我認為 AI 陪伴聊天 的操作很容易上手。",
        "我可以很容易的利用 AI 陪伴聊天 排解孤獨。",
        "我可以很容易的利用 AI 陪伴聊天 社交。",
        "我可以很容易的利用 AI 陪伴聊天 紓解壓力。",
        "我可以很容易的利用 AI 陪伴聊天 娛樂。",
      ],
      submitted: false,
      lock: false,
      textarea: "",
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
      await this.$axios.$post("/api/v1/final/tam", {
        userId: this.$route.query.id,
        tam: JSON.stringify(this.selected),
        other_options: textarea,
      });
    },
    next() {
      this.$router.push({
        path: "/posttest/finish",
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
