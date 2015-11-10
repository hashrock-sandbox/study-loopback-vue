<style>
  html, body{
    margin: 0;
    padding: 0;
    height: 100vh;
  }
  main{
    display: flex;
    height: 100%;
  }
  .browser__list{
    width: 150px;
    background: #666;
  }
  textarea{
    flex: 1;
    background: #333;
    color: white;
  }
  .preview{
    flex: 1;
  }
  .browser__list__new{
    width: 100%;
    margin: 0;
    height: 2rem;
  }
</style>

<template>
  <main>
    <aside class="browser__list">
      <button class="browser__list__new" @click="addPost">New</button>
      <browser-item v-for="post in posts" :post="post" @click="selectPost(post)" :editing-id="editing.id"></browser-item>
    </aside>
    <textarea v-model="editing.contents" @keyup="updatePost | debounce 500"></textarea>
    <div class="preview" v-text="editing.contents"></div>
  </main>
</template>

<script>
  var posts;
  /*  
  var mermaidAPI = require('mermaid').mermaidAPI;
  mermaidAPI.initialize({
      startOnLoad:false
  });
  Vue.filter("mermaid", function(value){
    return mermaidAPI.render(value);
  });
  */
  module.exports = {
    http: {
      root: "/api"
    },
    events: {
      "remove-post" : function(post){
        var self = this;
        if(confirm(post.title + "を削除しますか？")){
          posts.delete({id: post.id}, function(data, status, request){
            self.fetchPosts();
          });
        }
      }
    },
    data: function () {
      return {
        msg: 'Replaceing',
        posts: [],
        editing: {
          id: "",
          title: "",
          contents : ""
        }
      }
    },
    methods: {
      addPost : function(){
        var self = this;
        var title = window.prompt("Title:");
        if(title){
          posts.save({}, {title: title}, function(data, status, request){
            self.editing = data;
            self.fetchPosts();
          });
        }
      },
      updatePost : function(){
        if(this.editing.id || this.editing.id.length > 0){
          posts.update({id: this.editing.id}, this.editing, function(data, status, request){
            console.log("saved");
          })
        }
      },
      selectPost : function(post){
        this.editing = post;
      },
      fetchPosts : function(){
        var self = this;
        posts.query({}, function(items, status, request){
          self.posts = items;
        })
      }
    },
    ready: function(){
      posts = this.$resource('posts/:id');
      this.fetchPosts();
    },
    components: {
      browserItem: require("./browser-item.vue")
    }
  }
</script>