<template>
    <div class="container">
      <LoginForm/>
    </div>
</template>

<script>
import LoginForm from "@/components/LoginForm";
export default {
name: "Home",
  components: {
    LoginForm
  },
}
</script>

<style lang="scss">
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: calc(100% - 30px);
  padding: 15px;
}
</style>