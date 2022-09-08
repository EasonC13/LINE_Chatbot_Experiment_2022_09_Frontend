<template lang="">
  <div class="mx-2">
    <h3>Big5 人格測驗</h3>
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
  mounted() {
    console.log("this.$route", this.$route);
  },
  methods: {
    initSchema() {
      let newUISchema = {};
      let newSchema = {};
      this.questions.forEach((x, i) => {
        console.log(i, x);
        newSchema[i + 1] = {
          type: "number",
          title: x,
          enum: [1, 2, 3, 4, 5],
          enumNames: ["1", "2", "3", "4", "5"],
        };
        newUISchema[i + 1] = {
          "ui:widget": "RadioWidget",
          "ui:enumNames": ["1", "2", "3", "4", "5"],
        };
      });

      console.log("newUISchema", newUISchema);
      this.schema.properties = newSchema;
      this.uiSchema = newUISchema;
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
        this.next();
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
