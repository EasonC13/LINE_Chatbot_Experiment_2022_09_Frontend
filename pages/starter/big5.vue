<template lang="">
  <div class="mx-2">
    <h3>人格測驗</h3>
    <p>請選擇符合您個性的描述</p>
    <VueForm
      v-model="formData"
      :schema="schema"
      :uiSchema="uiSchema"
      :formFooter="formFooter"
      ref="Form"
    >
    </VueForm>
    <div class="text-center mb-2">
      <button @click="submit" class="btn btn-primary">
        已完成，進入下一個測驗
      </button>
    </div>
  </div>
</template>
<script>
//  使用
import VueForm from "@lljj/vue-json-schema-form";

export default {
  name: "Demo",
  emits: ["submit"],
  components: {
    VueForm,
  },
  watch: {
    formData() {
      console.log(this.formData);
    },
  },
  data() {
    return {
      formFooter: { show: false },
      formData: {},
      uiSchema: require("./big5UI.json"),
      schema: require("./big5.json"),
    };
  },
  beforeMount() {
    this.initForm();
  },
  mounted() {
    console.log("this.$route", this.$route);
  },
  methods: {
    initForm() {
      // for (let index = 1; index < 11; index++) {
      //   this.formData[index] = 4;
      // }
      // this.formData = { ...this.formData };
    },
    async submit() {
      let unfinish = [];
      this.schema.required.forEach((i) => {
        if (this.formData[i] == undefined) {
          unfinish.push(i);
        }
      });
      if (unfinish.length > 0) {
        alert(`題目 ${unfinish} 尚未完成`);
      } else {
        // POST
        let res = await this.$axios.$post("/api/v1/starter/big5", {
          userId: this.$route.query.id,
          big5: JSON.stringify(this.formData),
        });
        if (res.acknowledged) {
          this.next();
        } else {
          alert("發生未知錯誤，請稍後重試或聯絡管理員");
        }
      }
    },
    next() {
      this.$router.push({
        path: "/starter/attachment",
        query: { ...this.$route.query },
      });
    },
  },
};
</script>
<style lang=""></style>
