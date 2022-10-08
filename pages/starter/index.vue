<template lang="">
  <div class="container mt-5">
    <div class="">
      <h3>實驗說明</h3>
      <p>觀看以下影片進行實驗說明</p>
      <p>(如果不想看影片也可見下方文字版)</p>
      <iframe
        width="100%"
        src="https://www.youtube.com/embed/dQw4w9WgXcQ"
        title="YouTube video player"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
      ></iframe>
      <h3 class="mt-3">文字版說明</h3>
      <p>
        我們實驗的目的是要評估受試者與 AI
        聊天機器人對話的聊天體驗，接下來您將經歷五輪實驗，每輪流程如下：
      </p>
      <h4 id="1-">1. 好感度評估前測</h4>
      <p>在此請您紀錄您對此機器人的名字與頭像的感覺</p>
      <h4 id="2-">2. 十輪的聊天</h4>
      <p>
        在此階段您將會與不同設計的聊天機器人對話十輪，每次發送訊息算一輪。在您發送訊息後，4
        或 5 名 AI 聊天機器人將會對您的訊息做出回應。請留意，如果 AI
        重複回應同樣話語，代表系統卡住了，您可以輸入「換個話題」來切換聊天話題，改善聊天對象重複回應的情況。
      </p>
      <h4 id="3-">3. 好感度評估後測</h4>
      <p>
        在此請您紀錄您對此機器人的名字、頭像、聊天下來的感覺，並且提供一些您對他回應的主觀感受（形容詞）
      </p>
      <h4 id="4-">4. 聊天體驗量表</h4>
      <p>
        在此階段我們將使用可量化的量表來評估您的聊天體驗，請您記得使用「查看聊天記錄」功能回顧聊天過程並對聊天對象給予評價。
      </p>
      <h4 id="5-">5. 休息</h4>
      <p>
        在開始下一輪實驗前，我們系統會請您休息三分鐘，之後就會回到步驟 1
        開始下一輪實驗。
      </p>
      <h4 id="6-">6. 最終測試</h4>
      <p>
        最後我們會收取您對本實驗的心得，並且向您詢問基本個人資料及銀行帳號，用於發送實驗報酬。
      </p>
      <p>
        本實驗預計完成時間為 1
        小時，因為問卷題目較多，本實驗也準備相應豐厚的報酬（200元）給耐心完成實驗的您，敬請您耐心且確實地完成實驗。我們會使用比較多的問卷題目，是因為要跑一個比較複雜的統計分析來找出適合的聊天機器人設計，非常感謝您提供的寶貴實驗資料。
      </p>
      <p>通關密碼 2022</p>

      <form>
        <div class="form-group">
          <label for="exampleInputPassword1"
            >看完實驗說明後，請輸入通關密碼繼續</label
          >
          <input
            class="form-control"
            id="exampleInputPassword1"
            placeholder="影片最後的通關密碼"
            v-model="password"
          />
        </div>
      </form>

      <div class="text-center">
        <button @click="next" class="btn btn-primary mt-3">
          已閱讀並同意說明，開始實驗
        </button>
        <button @click="deny" class="btn btn-secondary mt-3">
          不同意參與實驗，停止並退出
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      password: "",
    };
  },
  mounted() {},
  methods: {
    deny() {
      this.$router.push({
        path: "/starter/deny",
        query: { ...this.$route.query },
      });
    },
    async next() {
      if (this.password != "2022") {
        alert("請閱讀完實驗說明或者觀看完實驗說明影片");
        return;
      }
      let res = await this.$axios.$get(
        `/api/v1/general/isfinish?userId=${this.$route.query.id}&condition=${this.$route.query.test}`
      );
      if (res.isfinish) {
        alert("您已經完成初始測驗，請直接關閉視窗回到聊天機器人繼續實驗");
        return;
      }
      await this.$axios.$post("/api/v1/starter/accept-term-of-test", {
        userId: this.$route.query.id,
        accept: true,
      });
      this.$router.push({
        path: "/starter/big5",
        query: { ...this.$route.query },
      });
    },
  },
};
</script>
<style lang=""></style>
