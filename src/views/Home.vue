<template>
  <div class="home">
    <CaptchaForm />
  </div>
</template>

<script>
// @ is an alias to /src
import CaptchaForm from "@/components/CaptchaForm.vue";

export default {
  name: "Home",
  components: {
    CaptchaForm
  }
};
</script>
