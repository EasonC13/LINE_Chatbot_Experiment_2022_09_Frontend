<template lang="">
  <div class="d-flex flex-row justify-content-start mb-4" style="opacity: 0.5">
    <img :src="image" alt="avatar 1" class="avatar" />
    <div>
      <p
        class="small p-2 ms-3 mb-1 lh-sm"
        style="background-color: #f5f6f7; border-radius: 20px"
      >
        <!-- <span class="font-weight-bold d-block">{{ name }}</span> -->
        {{ text }}
      </p>
    </div>
  </div>
</template>
<script>
export default {
  props: ["text", "image", "name"],
};
</script>

<style scoped>
.avatar {
  width: 45px;
  height: 100%;
  overflow: hidden;
  border-radius: 50%;
  position: relative;
}
</style>
