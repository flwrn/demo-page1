<template>
  <div class="loading" v-show="loading">
    <div class="loading__message">Loading...</div>
  </div>
</template>

<script>
export default {
  name: 'Loading',
  data() {
    return {
      loading: true,
    };
  },
  created() {
    setTimeout(() => {
      this.loading = false;
    }, 1000);
  },
};
</script>

<style scoped>
.loading {
  position: absolute;
  display: flex;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  justify-content: center;
  align-items: center;
  z-index: 10;
  background: #000000;
}
.loading__message {
  color: #ffffff;
}
</style>
