<template lang="">
  <div class="d-flex flex-row justify-content-start mb-4" style="opacity: 0.5">
    <img :src="image" alt="avatar 1" style="width: 45px; height: 100%" />
    <div>
      <p
        class="small p-2 ms-3 mb-1"
        style="background-color: ; border-radius: 20px"
      >
        {{ text }}
      </p>
    </div>
  </div>
</template>
<script>
export default {
  props: ["text", "image"],
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
