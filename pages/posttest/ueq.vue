<template lang="">
  <div class="" v-if="bots.length != 0 && !loading">
    <h3 class="mx-2">UEQ量表</h3>
    <div class="mx-2 sticky-top bg-white pb-1">
      <p>關於此聊天對象</p>
      <p>
        <img :src="bots[current_index].img_url" class="mx-3" />{{
          bots[current_index].name
        }}
      </p>

      <ChatHistory
        :condition="$route.query.test"
        :userId="$route.query.id"
        :current_bot_id="bots[current_index].id"
      ></ChatHistory>
      <p class="mt-3">根據聊天記錄，請問您對他的感覺？</p>
    </div>
    <div class="my-3" v-for="(question, q_num) in ueq" :key="q_num">
      <div class="mx-2 clearfix mb-0">
        <span class="float-left">{{ question.left }}</span>
        <span class="float-right">{{ question.right }}</span>
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
          style="width: 9vw"
          @click="select(q_num, num)"
          v-for="(num, index) in range"
          :key="index"
        >
          {{ num }}
        </button>
      </div>
    </div>

    <div class="text-center my-2">
      <button
        @click="previous"
        v-if="current_index != 0"
        class="btn btn-secondary"
      >
        上一位
      </button>
      <button
        @click="submit"
        class="btn btn-primary"
        v-if="current_index + 1 != bots.length"
        :disabled="!form_finish"
      >
        下一位
      </button>
      <button
        @click="submit"
        class="btn btn-primary"
        v-else
        :disabled="!form_finish"
      >
        完成
      </button>
    </div>
  </div>
  <div v-else>
    <p class="text-center mt-5">Loading...</p>
  </div>
</template>
<script>
//  使用
import VueForm from "@lljj/vue-json-schema-form";
import { init } from "events";
import ueq from "./ueq";
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
      selected: Array(ueq.length).fill(1),
      range: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      ueq: ueq,
      current_UEQ: [],
      all_ueq: {},
      submitted: false,
      lock: false,
    };
  },
  computed: {
    condition() {
      let [a, b, _] = this.$route.query.test.split("_");
      return `${a}_${b}`;
    },
    form_finish() {
      let res = this.selected.filter((x) => x == 0);
      return res.length == 0;
    },
  },
  async beforeMount() {
    this.bots = await this.$axios.$get(
      `/api/v1/pretest/bots?condition=${this.condition}`,
      {
        userId: this.$route.query.id,
        style: this.selected,
      }
    );
  },
  methods: {
    select(q_num, i) {
      this.selected[q_num] = i;
      this.selected = [...this.selected];
    },
    previous() {
      this.save_current();
      let item = this.all_ueq[this.current_index - 1];
      this.current_index--;
      this.reinit();
      this.selected = item.rating;
    },
    save_current() {
      this.all_ueq[this.current_index] = {
        id: this.bots[this.current_index].id,
        name: this.bots[this.current_index].name,
        img: this.bots[this.current_index].img_url,
        rating: this.selected,
      };
    },
    async submit() {
      if (this.lock) {
        return 0;
      } else {
        this.lock = true;
        if (this.selected == 0) {
          alert("請選擇一個最適合的好感度並輸入感覺，再點選繼續");
        } else {
          this.save_current();
          if (this.current_index + 1 == this.bots.length) {
            if (!this.submitted) {
              this.submitted = true;
              await this.$axios.$post("/api/v1/posttest/ueq", {
                userId: this.$route.query.id,
                condition: this.condition,
                status: this.$route.query.test,
                all_ueq: JSON.stringify(this.all_ueq),
              });
              this.next();
            }
          } else {
            this.current_index++;
            this.reinit();
            let item = this.all_ueq[this.current_index];
            if (item) {
              this.selected = item.rating;
              this.textarea = item.comment;
            }
          }
        }
        this.lock = false;
      }
    },
    reinit() {
      this.selected = Array(this.ueq.length).fill(1);
      this.loading = true;
      let vue = this;
      setTimeout(() => {
        vue.loading = false;
      }, 10);
    },
    next() {
      this.$router.push({
        path: "/posttest/ueq_intro",
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
