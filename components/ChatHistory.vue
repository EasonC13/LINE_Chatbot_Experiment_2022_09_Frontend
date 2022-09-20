<template lang="">
  <div>
    <!-- Button trigger modal -->
    <div class="">
      <button
        type="button"
        class="btn btn-primary"
        data-toggle="modal"
        data-target="#exampleModal"
      >
        查看___的聊天記錄
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
            <h5 class="modal-title" id="exampleModalLabel">聊天記錄回顧</h5>
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
                        <chatOwnMessage
                          :text="text"
                          :image="image"
                        ></chatOwnMessage>

                        <chatTargetBot
                          :text="text"
                          :image="image"
                        ></chatTargetBot>
                        <chatOtherBot
                          :text="text"
                          :image="image"
                        ></chatOtherBot>
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
  data() {
    return {
      text: "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Aliquid velit pariatur vel rerum earum molestias nam cumque, dignissimos itaque voluptate deleniti laboriosam! Eius quia doloremque excepturi aliquid et placeat quibusdam.",
      image:
        "https://mdbcdn.b-cdn.net/img/Photos/new-templates/bootstrap-chat/ava3-bg.webp",
    };
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
