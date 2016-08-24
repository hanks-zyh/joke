<template>
  <div id="app" class="main">
      <ul>
          <li class="joke" v-for="joke in jokeList">
              <img class="avatar" :src="joke.avatar" alt="">
              <div class="username">{{joke.user_name}}</div>
              <p>{{ joke.title }}</p>
              <div class="pic">
                  <img :src="joke.media_data[0].origin_img_url.origin_pic_url" alt="">
                  <div class="expand" @click="expandImage($index)">点击查看全图</div>
              </div> 
          </li>  
      </ul>
      <div v-show="jokeList.length>0" class="btn-more" @click="nextPage">加载更多</div>
  </div>
</template>

<script>
var Vue = require('vue')
var vueResource = require('vue-resource')
Vue.use(vueResource)
export default {
  data () {
    return {
      jokeList: [],
      maxPos: ''
    }
  },
  created () {
    console.log('ready')
    this.maxPos = new Date().getTime()
    this.getData(this.maxPos)
  },
  methods :{
    getData (maxPos) {
      var url = 'http://napi.uc.cn/3/classes/topic/lists/%E9%A6%96%E9%A1%B5%E7%B2%BE%E9%80%89%E5%88%97%E8%A1%A8?_app_id=hottopic&_size=10&_fetch=1&_fetch_incrs=1&_max_pos='+maxPos+'&_fetch_total=1&_select=like_start%2Cdislike_start%2Ctitle%2Ctag%2Cmedia_data%2Clist_info%2Ccontent%2Cavatar%2Cuser_name%2Cis_hot%2Chot_comment%2Ccomment_total%2Coriginal%2Ctv_duration';
      this.$http.get(url).then((response) => {
        var json = JSON.parse(response.body)
        var list = json.data 
        for (var i = 0; i < list.length; i++) {
          this.jokeList.push(list[i])
        } 
      }, (error) => {
        console.error(error)
      })
    },
    nextPage () {
        this.maxPos = this.jokeList[this.jokeList.length-1]._pos
        this.getData(this.maxPos)
    },
    expandImage(index){
       this.jokeList[index].media_data[0].wifi_img_url = this.jokeList[index].media_data[0].origin_img_url.origin_pic_url;
    }
  }
}
</script>

<style>
*{
    padding: 0;
    margin: 0;
}
body {
  font-family: Helvetica, sans-serif;
  background-color: #e1e1e1;
}
ul{
    list-style: none;
}
.main{
    width: 600px;
    margin: 0 auto;
}

.joke{
    background-color: #ffffff;
    overflow: hidden;
    margin:20px 40px;
}

.avatar{
    margin: 1em;
    width: 30px;
    height: 30px;
    -webkit-border-radius: 100%;
    -webkit-border-radius: 100%;
    border-radius: 100%;
    margin-right: 10px;
    background: url(http://image.uc.cn/s/uae/g/0q/product/default-avatar.png) no-repeat;
    background-size: 30px 30px;
}
.username{
    margin: 1em 0;
    display: inline-block;
    font-size: 1em; 
    line-height: 30px;
    color: #888;
    -webkit-box-flex: 1;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}
.joke>p{
    margin: 0  1em 1em;
    font-size: 1.1em;
    color: #4e4e4e;
    line-height: 1.5em;
    word-break: break-all;
}
.pic{
    position: relative;
    /*height: 480px;*/
}
.pic>img{
    width: 100%; 
}
.expand{
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 1;
    margin: 0;
    background: rgba(0,0,0,.5);
    padding: 14px 0;
    color: #fff;
    text-align: center;
    display: none;
}
.btn-more{
    width: 87%;
    display: flex;
    height: 8em;
    margin: 0 auto;
    background: none;
    border: none;
    font-size: 1em;
    justify-content: center;
    align-items: center;
}

.btn-more:hover{
    background: #eeeeee;
}
</style>
