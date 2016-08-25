<template>
  <div class="overlay" :style="{ height: joke.showCommentList ? '100%' : '0%' }">
    <ul>
      <li v-for="(comment, index) in joke.commentList ">
           <div class="comment-item">
             <h2>{{comment.author}}</h2>
             <p>{{comment.message}}</p>
           </div>
      </li>
    </ul>
  </div>
</template>
<script> 

var Vue = require('vue')
var vueResource = require('vue-resource')
Vue.use(vueResource)

export default {

  name: 'CommentList',

  props:{
    joke: Object,
  }

}
</script>

<style>
.overlay {
    width: 100%;
    color: white;
    background-color: rgba(0,0,0, 0.7);
    overflow-y: hidden;
    transition: 0.4s;
}
 
.comment-item{
  padding: 0.8em 0.8em;
  display: flex;
  flex-direction: row;
  border-bottom: 1px dashed #888888;
}
.comment-item>h2{
  color: #b3b3b3;
  font-size: 0.2em;
  width: 5em;
  text-overflow: ellipsis;
  overflow: hidden;
  margin-right: 1em;
}
.comment-item>p{
  font-size: 0.8em;
  line-height: 1.2em;
}

</style>
