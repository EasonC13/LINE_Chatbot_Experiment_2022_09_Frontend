<template lang="">
  <div>
    <div class="d-flex flex-row justify-content-end mb-4 pt-1">
      <div>
        <p
          class="small p-2 me-3 mb-1"
          style="background-color: #86d97b; border-radius: 20px"
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
