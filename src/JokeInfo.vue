<template>
  <div class="joke-info">
    <div>👍&nbsp;&nbsp;{{ joke._incrs.like }}</div>
    <div>👎&nbsp;&nbsp;{{ joke._incrs.dislike }}</div>
    <div class="comment-count" @click="showCommentList">🙉&nbsp;&nbsp;{{ joke.comment_total }}</div>
  </div>
</template>
<script> 
export default {

  name: 'JokeInfo',

  props:{
    joke: Object
  },

  data () {
    return {
    }
  },
  created () {
  },
  methods: {
    showCommentList () {
      this.$parent.currentJoke = this.joke
      this.$parent.openCommentList = true
    }
  },

  events: {
    'showCommentList': function (joke) {
      console.log("click:"+this.joke)
       
    },
    'hideCommentList': function () {
    }
  }
}
</script>

<style>
.joke-info{
  height: 3em;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  padding-right: 1em;
}
.joke-info>div{
  margin-left: 2em;
}
.comment-count{
  cursor: pointer;
}
</style>
