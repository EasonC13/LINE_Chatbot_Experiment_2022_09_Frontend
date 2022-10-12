<template lang="">
  <div class="mx-2 mt-3">
    <h3>個人資料收集</h3>
    <form>
      <div class="form-group">
        <label for="exampleFormControlInput1">請問您的信箱（用於發送轉帳通知）</label>
        <input
          type="email"
          class="form-control"
          id="exampleFormControlInput1"
          placeholder="name@example.com"
          v-model="email"
        />
      </div>
      <div class="form-group">
        <label for="exampleFormControlSelect1">請問您的生理性別：</label>
        <select
          class="form-control"
          id="exampleFormControlSelect1"
          v-model="gender"
        >
          <option>男</option>
          <option>女</option>
          <option>不願透露</option>
        </select>
      </div>
      <div class="form-group">
        <label for="exampleFormControlInput1">請問您的年齡</label>
        <input
          type="number"
          class="form-control"
          id="exampleFormControlInput1"
          v-model="age"
        />
      </div>
      <div class="form-group">
        <label for="exampleFormControlInput1"
          >您的銀行代號（如郵局為 700）</label
        >
        <input
          type="number"
          class="form-control"
          id="exampleFormControlInput1"
          v-model="bank_id"
        />
      </div>
      <div class="form-group">
        <label for="exampleFormControlInput1">您的銀行帳號（14碼）</label>
        <input
          type="number"
          class="form-control"
          id="exampleFormControlInput1"
          v-model="bank_account"
        />
      </div>
      <p>*請確認銀行帳號正確無誤，這樣受試者報酬才能順利匯入</p>
      <br></br>
      <div class="form-group">
        <label for="exampleFormControlTextarea1">備註（如無請留白）</label>
        <textarea
          class="form-control"
          id="exampleFormControlTextarea1"
          rows="3"
          v-model="feedback"
        ></textarea>
      </div>
    </form>
    <div class="text-center mb-2">
      <button
        @click="submit"
        class="btn btn-primary"
        :disabled="!(email && gender && age && bank_id && bank_account)"
      >
        已完成全部資料，開始實驗
      </button>
    </div>
  </div>
</template>
<script>
import VueForm from "@lljj/vue-json-schema-form";

export default {
  name: "Demo",
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
      uiSchema: {},
      email: "",
      gender: "",
      age: "",
      bank_id: "",
      bank_account: "",
      feedback: "",
    };
  },
  mounted() {
    console.log("this.$route", this.$route);
  },
  methods: {
    async submit() {
      await this.$axios.$post("/api/v1/final/final", {
        userId: this.$route.query.id,
        email: this.email,
        gender: this.gender,
        age: this.age,
        bank_id: this.bank_id,
        bank_account: this.bank_account,
        feedback: this.feedback,
      });
      this.next();
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
<style lang=""></style>
