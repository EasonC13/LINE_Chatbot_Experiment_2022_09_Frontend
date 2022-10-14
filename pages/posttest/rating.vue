<template lang="">
  <div class="mx-2" v-if="bots.length != 0 && !loading">
    <div class='dummy_bots' v-for="(bot, index) in bots" :key="'dummy_bots_' + index" v-show="false">
      <img :src="bot.img_url"></img>
    </div>
    <h3>好感度評估後測</h3>
    <div class="pb-4">
      <p>關於此聊天對象</p>
      <p>
        <img :src="bots[current_index].img_url" class="mx-3 w-25 h-25" />{{
          bots[current_index].name
        }}
      </p>

      <ChatHistory
        :condition="$route.query.test"
        :userId="$route.query.id"
        :current_bot_id="bots[current_index].id"
        :bots="bots"
      ></ChatHistory>
    </div>
    <div class="mb-3">
      <p>請問您的好感分數？</p>
      <div class="text-center">
        <button
          class="btn"
          type="button"
          :class="{
            'btn-primary': selected == index + 1,
            'btn-outline-dark': !(selected == index + 1),
          }"
          @click="select(index + 1)"
          v-for="(text, index) in range"
          :key="index"
        >
          {{ text }}
        </button>
      </div>
    </div>
    <!-- <div>
      <p class="mb-0">請問您跟他聊天的感覺？</p>
      <p>（形容詞，使用空白間隔，開放式回應，字數不限）</p>
      <div class="input-group">
        <textarea
          v-model="textarea"
          class="form-control"
          aria-label="With textarea"
        ></textarea>
      </div>
    </div> -->

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
        :disabled="!(selected != 0 && textarea.length != 0)"
      >
        下一位
      </button>
      <button
        @click="submit"
        class="btn btn-primary"
        v-else
        :disabled="!(selected != 0 && textarea.length != 0)"
      >
        完成
      </button>
      <p
        class="text-secondary mt-3"
        v-if="!(selected != 0 && textarea.length != 0)"
      >
        需先選擇好感度才能繼續
      </p>
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
      selected: 0,
      range: [1, 2, 3, 4, 5, 6, 7],
      cost_time: Array(5).fill(0),
      current_start_time: 0,
      ratings: {},
      submitted: false,
      lock: false,
      textarea: "因為擔心受試者太累所以取消",
    };
  },
  computed: {
    condition() {
      let [a, b, _] = this.$route.query.test.split("_");
      return `${a}_${b}`;
    },
  },
  async beforeMount() {
    this.bots = await this.$axios.$get(
      `/api/v1/general/bots?condition=${this.condition}`
    );
  },
  mounted() {
    this.current_start_time = performance.now();
  },
  methods: {
    select(i) {
      if (i == this.selected) {
        this.selected = 0;
      } else {
        this.selected = i;
      }
    },
    previous() {
      this.save_current();
      let item = this.ratings[this.current_index - 1];
      this.current_index--;
      this.reinit();
      this.selected = item.rating;
      this.textarea = item.comment;
    },
    save_current() {
      let time_cost = (performance.now() - this.current_start_time) / 1000;
      this.cost_time[this.current_index] += time_cost;
      this.ratings[this.current_index] = {
        id: this.bots[this.current_index].id,
        name: this.bots[this.current_index].name,
        img: this.bots[this.current_index].img_url,
        rating: this.selected,
        comment: this.textarea,
      };
    },
    async submit() {
      if (this.lock) {
        return 0;
      } else {
        this.lock = true;
        if (this.selected == 0 || this.textarea.length == 0) {
          alert("請選擇一個最適合的好感度並輸入感覺，再點選繼續");
        } else {
          this.save_current();
          if (this.current_index + 1 == this.bots.length) {
            if (!this.submitted) {
              this.submitted = true;
              await this.$axios.$post("/api/v1/posttest/ratings", {
                userId: this.$route.query.id,
                condition: this.condition,
                status: this.$route.query.test,
                ratings: JSON.stringify(this.ratings),
                cost_time: JSON.stringify(this.cost_time),
              });
              this.next();
            }
          } else {
            this.current_index++;
            this.reinit();
            let item = this.ratings[this.current_index];
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
      this.selected = 0;
      this.loading = true;
      this.textarea = "因為太麻煩而跳過";
      let vue = this;
      setTimeout(() => {
        vue.loading = false;
        vue.current_start_time = performance.now();
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
