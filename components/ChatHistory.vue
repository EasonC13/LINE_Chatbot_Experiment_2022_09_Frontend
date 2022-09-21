<template lang="">
  <div class="clearfix">
    <!-- Button trigger modal -->
    <div class="">
      <button
        type="button"
        class="btn btn-primary"
        data-toggle="modal"
        data-target="#exampleModal"
      >
        查看 {{ current_bot.bot_name }} 的聊天記錄
      </button>
    </div>

    <!-- Modal -->
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <p class="h6 modal-title" id="exampleModalLabel">
              與 {{ current_bot.bot_name }} 的聊天回顧
            </p>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body clearfix">
            <section>
              <div class="">
                <div class="row d-flex justify-content-center overflow-auto">
                  <div class="col-md-10 col-lg-8 col-xl-6">
                    <div class="" id="chat2">
                      <div
                        class=""
                        data-mdb-perfect-scrollbar="true"
                        style="position: relative; height: 70vh"
                      >
                        <div v-for="(item, index) in history" :key="index">
                          <chatOwnMessage
                            :text="item.input_text"
                            :image="''"
                          ></chatOwnMessage>
                          <div
                            v-for="(item_, index_) in item.responses"
                            :key="index_"
                          >
                            <chatTargetBot
                              :text="item_.response_text"
                              :image="item_.bot_img"
                              :name="item_.bot_name"
                              v-if="current_bot_id == item_.bot_id"
                            ></chatTargetBot>
                            <chatOtherBot
                              :text="item_.response_text"
                              :image="item_.bot_img"
                              :name="item_.bot_name"
                              v-else="current_bot_id == item_.bot_id"
                            ></chatOtherBot>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </section>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-dismiss="modal"
            >
              關閉
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["userId", "condition", "current_bot_id"],
  data() {
    return {
      text: "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Aliquid velit pariatur vel rerum earum molestias nam cumque, dignissimos itaque voluptate deleniti laboriosam! Eius quia doloremque excepturi aliquid et placeat quibusdam.",
      image:
        "https://mdbcdn.b-cdn.net/img/Photos/new-templates/bootstrap-chat/ava3-bg.webp",
      history: [],
    };
  },
  computed: {
    current_bot() {
      if (this.history.length == 0) {
        return "___";
      } else {
        let current = this.history[0].responses.filter(
          (x) => x.bot_id == this.current_bot_id
        );
        return current[0];
      }
    },
  },
  async mounted() {
    let condition = this.condition.split("_");
    condition.pop();
    condition = condition.join("_");
    let res = await this.$axios.$get(
      `/api/v1/posttest/chat_history?userId=${this.userId}&condition=${condition}`
    );
    this.history = res.chats;
  },
};
</script>
<style scoped>
#chat2 .form-control {
  border-color: transparent;
}

#chat2 .form-control:focus {
  border-color: transparent;
  box-shadow: inset 0px 0px 0px 1px transparent;
}

.divider:after,
.divider:before {
  content: "";
  flex: 1;
  height: 1px;
}
</style>
<style>
.modal-backdrop {
  z-index: -1;
}
</style>
