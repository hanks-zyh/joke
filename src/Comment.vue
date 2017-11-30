<template>
  <div v-if="show" class="hot-comment">
      <div class="hot-tag">神评论</div><div class="co-username">  {{ hotComment.author }}</div>
      <p>{{ hotComment.message }}</p>
  </div>
</template>
<!-- 
"author": "氵炎╰_╯宀莫"
"message": "建国以后动物不许成精，不许成精说了多少次了"
"like": 93
"dislike": 0
"_id": "39080426"
"extra": {
"avatar": "http://image.uc.cn/o/uop/1Ht08/;;0,uop/g/uop/avatar/16070208317cbea030ab48958ed742cdd88909552c.jpg;3,160"
"post_title": "别过来。有陷阱。"
"post_type": "detail"
-->
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
    isplay: inline-block;
    border: 1px solid #d07979;
    padding: 0.1em 0.2em;
    color: #8a9076;
}
.co-username{
    display: inline;
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
