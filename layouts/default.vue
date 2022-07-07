<template>
  <div>
    <div>
      <AppHeader></AppHeader>
    </div>
    <div id="index-Body">
      <div id="App-Menu">
        <AppMenu></AppMenu>
      </div>
      <div id="main">
        <Nuxt />
      </div>
    </div>
    <div>
      <AppFooter></AppFooter>
    </div>
  </div>
</template>
<script>
  import AppHeader from '../components/AppHeader.vue';
  import AppMenu from '../components/AppMenu.vue';
  import AppFooter from '../components/AppFooter.vue';
  export default {
    components: { 
        AppHeader,
        AppMenu,
        AppFooter,
      },
  }
</script>
