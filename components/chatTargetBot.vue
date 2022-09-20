<template lang="">
  <div>
    <div class="d-flex flex-row justify-content-start mb-4">
      <img :src="image" alt="avatar 1" style="width: 45px; height: 100%" />
      <div>
        <p
          class="small p-2 ms-3 mb-1"
          style="background-color: #f5f6f7; border-radius: 20px"
        >
          {{ text }}
        </p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["text", "image"],
};
</script>
<style lang=""></style>
