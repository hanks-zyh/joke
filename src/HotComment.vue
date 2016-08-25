<template>
  <div v-if="show" class="hot-comment">
      <div class="hot-tag">神评论</div><div class="username">  {{ hotComment.author }}</div>
      <p>{{ hotComment.message }}</p>
  </div>
</template>
<script> 
export default {

  name: 'HotComment',

  props:{
    hotComment: Object
  },

  data () {
    return {
       show: false
    }
  },
  created () {
    this.show = this.hotComment && this.hotComment.hasOwnProperty('message');
  },
}
</script>

<style>
.hot-comment {
    background-color: #eaeaea;
    margin: 10px 6px;
    padding: 6px 10px 13px;
    border-radius: 3px;
    line-height: 20px;
    font-size: 13px;
}
.hot-tag{
    display: inline-block;
    border: 1px solid #d07979;
    padding: 0 4px;
    color: #d07979;
}
.username{
    display: inline-block;
    margin: 1em 0;
    font-size: 1em; 
    color: #888;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}
.hot-comment>p{
  font-size: 1.1em;
} 
</style>
