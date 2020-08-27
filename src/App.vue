<template>
  <div id="app">
    <FrontPage/>
    <Loading/>
  </div>
</template>

<script>
import FrontPage from '@/components/FrontPage.vue';
import Loading from '@/components/Loading.vue';

export default {
  name: 'App',
  components: {
    FrontPage,
    Loading,
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  background: rgb(80, 80, 80);
}
body, body * {
  box-sizing: border-box;
}
</style>
