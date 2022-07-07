<template>
  <div>
    <BlogCreateEdit/>
  </div>
</template>
<script>
  import BlogCreateEdit from '../components/blog/BlogCreateEdit.Vue';
  export default {
      name: "CreateEditPage",
      components: { 
        BlogCreateEdit,
      }
  };
  require('../assets/css/main.css');
</script>
<style>

</style>