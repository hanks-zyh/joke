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
      console.log("click:"+this.joke)
      this.$dispatch('showCommentList', this.joke)
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
