<template>
  <div>
    <BlogList :listBlog = "listBlog" :listPosition="listPosition"></BlogList>
  </div>
</template>
<script>
  import BlogList from '../components/blog/BlogList.Vue';
  export default {
      name: "IndexPage",
      components: { 
        BlogList
      },
      data(){
          return{
            currentId: 0,
            listBlog: [],
            listPosition: [],
          };
      },
      methods: {
        
      },
      async beforeMount(){
        this.listBlog = await fetch('https://localhost:44302/api/Blogs')
            .then((res)=> res.json());
        this.listPosition = await fetch('https://localhost:44302/api/PositionItems')
            .then((res)=>res.json());
      } 
  };
</script>
<style>

</style>


