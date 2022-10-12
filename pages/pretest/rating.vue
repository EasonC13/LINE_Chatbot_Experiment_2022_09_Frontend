<template lang="">
  <div class="mx-2" v-if="bots.length != 0 && !loading">
    <h3>好感度評估前測</h3>
    <p>關於此聊天對象</p>
    <p>
      <img :src="bots[current_index].img_url" class="mx-3 w-25 h-25" />{{
        bots[current_index].name
      }}
    </p>
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

    <div class="text-center my-2" v-if="selected != 0">
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
      >
        下一位
      </button>
      <button @click="submit" class="btn btn-primary" v-else>完成</button>
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
    };
  },
  computed: {
    condition() {
      let [a, b, _] = this.$route.query.test.split("_");
      return `${b}`;
    },
  },
  async beforeMount() {
    this.bots = await this.$axios.$get(
      `/api/v1/general/bots?condition=${this.condition}`,
      {
        userId: this.$route.query.id,
        style: this.selected,
      }
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
        if (this.selected == 0) {
          alert("請選擇一個最適合的好感度，再點選繼續");
        } else {
          this.save_current();
          if (this.current_index + 1 == this.bots.length) {
            if (!this.submitted) {
              this.submitted = true;
              await this.$axios.$post("/api/v1/pretest/ratings", {
                userId: this.$route.query.id,
                condition: this.condition,
                status: this.$route.query.test,
                ratings: JSON.stringify(this.ratings),
                cost_time: JSON.stringify(this.cost_time),
              });
              this.$router.push({
                path: "/pretest/finish",
                query: { ...this.$route.query },
              });
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
      let vue = this;
      setTimeout(() => {
        vue.loading = false;
        vue.current_start_time = performance.now();
      }, 10);
    },
    next() {
      this.$router.push({
        path: "/starter/finish",
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
  width: 200px;
  height: 200px;
  overflow: hidden;
  border-radius: 50%;
}
.circular--portrait img {
  width: 100%;
  height: auto;
}
</style>
