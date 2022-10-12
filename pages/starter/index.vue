<template lang="">
  <div class="container mt-5">
    <div class="">
      <h3>實驗說明</h3>
      <p>
        我們實驗的目的是要評估受試者與 AI
        聊天機器人對話的聊天體驗，接下來您將經歷以下實驗流程：
      </p>
      <h4 id="0-">0. 基本資料收集</h4>
      <p>在此階段我們會收集您的基本資料。</p>
      <h4 id="1-">1. 好感度評估前測</h4>
      <p>在此階段請您紀錄您對即將聊天的對象的名字與頭像的感覺</p>
      <h4 id="2-">2. 十輪的聊天</h4>
      <p>
        在此階段您將會與不同設計的聊天機器人對話十輪，在您發送訊息後，5 名 AI
        聊天機器人將會對您的訊息做出回應。此階段有幾個注意事項：
      </p>
      <ol>
        <li>
          <p>
            每次發送訊息算一輪，請您把所有要打的內容放在同一個訊息內發出。如必要可以使用換行發送較長的訊息。
            <img src="https://i.imgur.com/pmCfnV0.png" alt="" width="100%" />
          </p>
        </li>
        <li>
          <p>
            如果 AI
            重複回應同樣話語或出現奇怪回應，代表系統卡住了，您可以選擇「刷新」來刷新，改善聊天對象重複回應的情況。
          </p>
        </li>
        <p><img src="https://i.imgur.com/1eXY7Ig.png" alt="" width="100%" /></p>

        <li>
          如果覺得您接下來的發言是在回應特定聊天對象，強烈建議您使用標記功能「@XXX」來標記那個機器人，只要在送出訊息前選擇聊天列表上方的「@」即可，方便我們做數據分析使用。
        </li>

        <li>
          關於聊天話題的選擇，基本上沒有限制。我們使用的是最先進的聊天引擎，會根據你的發言做出反應，可以陪你天南地北的聊任何話題。
        </li>
        <li>只能使用純文字訊息，無法接收貼圖、表情符號。</li>
      </ol>
      <h4 id="3-">3. 好感度評估後測</h4>
      <p>在此請您紀錄您對此機器人的名字、頭像、聊天下來的感覺。</p>
      <h4 id="4-">4. 聊天體驗量表</h4>
      <p>
        在此階段我們將使用可量化的量表來評估您對每個聊天對象的聊天體驗，請您記得使用「查看聊天記錄」功能回顧聊天過程並對聊天對象給予評價。
      </p>
      <h4 id="5-">5. 休息</h4>
      <p>
        在開始下一輪實驗前，我們系統會請您休息三分鐘，要自主休息更久或直接跳過也可以。之後就會回到步驟
        1 開始下一輪實驗。總共有四輪這樣的實驗。
      </p>
      <h4 id="6-">6. 最終資料填寫</h4>
      <p>最後我們會收取您對本實驗整體的量化體驗與心得。</p>
      <h4 id="-">注意事項</h4>
      <ul>
        <li>建議您使用手機進行實驗，模擬真實的聊天情況。</li>
        <li>
          本實驗預計完成時間為 1.5
          小時，因為要不斷操作手機，且問卷題目較多，本實驗也準備相應豐厚的報酬（300元）給耐心完成實驗的您，敬請您耐心且確實地完成實驗。我們會使用比較多的問卷題目，是因為要跑一個比較複雜的統計分析來找出適合的聊天機器人設計，非常感謝您耐心提供的寶貴實驗資料。
        </li>
        <li>
          實驗途中可任意暫停休息，只要在期限內完成即可，有任何問題或不適請立刻聯絡實驗負責人陳怡升，聯絡資訊在
          LINE 上的第一則訊息。
        </li>
      </ul>

      <!-- <p>通關密碼 2022</p>

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
      </form> -->

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
      password: "2022",
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
