
<template>
  <div>
    <div>
      <BlogSearchForm @newKeySearch="newKeySearch"></BlogSearchForm>
    </div>
    <BlogList :listBlog = "listBlogSearch" :listPosition="listPosition"></BlogList>
  </div>
</template>
<script>
  import AppHeader from '../components/AppHeader.vue';
  import AppFooter from '../components/AppFooter.vue';
  import AppMenu from '../components/AppMenu.vue';
  import BlogList from '../components/blog/BlogList.Vue';
  import BlogCreateEdit from '../components/blog/BlogCreateEdit.Vue';
  import BlogSearchForm from '../components/blog/BlogSearchForm.Vue';
  export default {
      name: "SearchPage",
      components: { 
        AppHeader,
        AppMenu,
        AppFooter,
        BlogList,
        BlogCreateEdit,
        BlogSearchForm,
      },
      async beforeMount(){
        this.listBlog = await fetch('https://localhost:44302/api/Blogs')
            .then((res)=> res.json());
        this.listPosition = await fetch('https://localhost:44302/api/PositionItems')
            .then((res)=>res.json());
        this.listBlogSearch=this.listBlog;
      },
      data(){
          return{
              keySearch: '',
              listBlog: [],
              listBlogSearch:[],
              listPosition:[],
          };
      },
      methods: {
        /// <summary>
        /// method newKeySearch(newKey) to edit keySearch and change this listBlog by key Search
        /// </summary>
        /// <param name="newKey"></param>
        /// <returns></returns>
        newKeySearch(newKey){
          this.keySearch= newKey.toLowerCase();
          this.listBlogSearch= this.listBlog.filter(function(blog){
            return blog.title.toLowerCase().includes(newKey.toLowerCase());
          })
        },
      }
  };
  require('../assets/css/main.css');
</script>
<style>

</style>


