<template>
  <div v-theme:column="'narrow'" id="show-blogs">
    <h1>All Blogs</h1>
    <input type="text" v-model="search" placeholder="Search blogs">
    <div v-for="blog in filteredBlogs" class="single-blog">
      <router-link v-bind:to="'/blog/' + blog.id"><h3 v-rainbow>{{blog.title | to-uppercase}}</h3></router-link>
      <article>{{blog.content | snippet}}</article>
    </div>
  </div>
</template>

<script>
import searchMixin from '../mixins/searchMixin';
export default {
  data(){
    return {
      blogs: [],
      search: ''
    }
  },
  methods: {
  
  },
  computed:{
    
  },
  created() {
    this.$http.get('https://vue-blog-d136a.firebaseio.com/posts.json').then(function(data){
      return data.json()
    }).then(function(data){
      var blogsArray = [];
      for(let key in data){
        data[key].id = key
        blogsArray.push(data[key]);
      }
      this.blogs = blogsArray;
    }) 
  },
  filters:{
    'to-uppercase':function(value){
        return value.toUpperCase();
      }
  },
  directives:{
    'rainbow':{
      bind(el, binding, vnode){
      el.style.color = "#" + Math.random().toString().slice(2,8);
      }
    }
  },
  mixins: [searchMixin]

}

</script>

<style>
#show-blogs{
    max-width: 800px;
    margin: 0px auto;
}
.single-blog{
    padding: 20px;
    margin: 20px 0;
    box-sizing: border-box;
    background: #eee;
}
</style>
