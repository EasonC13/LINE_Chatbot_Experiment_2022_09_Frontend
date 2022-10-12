<template lang="">
  <div class="container mt-5">
    <div class="">
      <h3>End</h3>
      <p>
        已完成本階段所有流程，請在關閉視窗後回到聊天機器人並點選「下一步」繼續實驗
      </p>
      <div class="text-center">
        <button @click="next" class="btn btn-primary">關閉視窗</button>
      </div>
      <hr class="mt-3"></hr>
      <h4>關於聊天階段的提示</h4>
      <p>
        等等您將會與不同設計的聊天機器人對話十輪，在您發送訊息後，5 名 AI
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
      </ol>
      <div class="text-center mb-3">
        <button @click="next" class="btn btn-primary">關閉視窗</button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {};
  },
  mounted() {},
  methods: {
    next() {
      window.location = "https://line.me/R/oaMessage/@956unykg/?";
    },
  },
};
</script>
<style lang=""></style>
