<template>
  <div id="app">
    <div class="main">
      <ul>
        <li class="joke" v-for="(joke, index) in jokeList">
          <img class="avatar" :src="joke.avatar" alt="">
          <div class="username">{{joke.user_name}}</div>
          <p>{{ joke.title }}</p>
          <div class="pic" :style="{ height: joke.needExpand ? expandHeight +'px' : '100%' }">
            <img :src="joke.origin_img_url" @click="clickImg(index)" alt="">
            <div class="expand" v-show="joke.needExpand" @click="expandImage(index)">点击查看全图</div>
          </div>
          <hot-comment :hot-comment="joke.hot_comment"></hot-comment>
          <joke-info :joke="joke"></joke-info>
          <comment-list :joke="joke"></comment-list>
        </li>
      </ul>
      <div v-show="jokeList.length>0" class="btn-more" @click="nextPage">加载更多</div>
    </div>
  </div>
</template>

<script>
import HotComment from './HotComment.vue';
import JokeInfo from './JokeInfo.vue';
import CommentList from './CommentList.vue';

var Vue = require('vue');
var vueResource = require('vue-resource');
Vue.use(vueResource);
export default {
  components: {
    HotComment,
    JokeInfo,
    CommentList
  },

  data () {
    return {
      expandHeight: 800,
      jokeList: [],
      maxPos: '',
      urlSet: undefined,
      openCommentList: false,
      currentJoke: {}
    }
  },
  created () {
    this.urlSet = new Set();
    this.maxPos = new Date().getTime();
    this.getData(this.maxPos);
    this.checkLoadMore();
  },
  methods :{
    getData (maxPos) {
      var url = 'http://napi.uc.cn/3/classes/topic/lists/%E9%A6%96%E9%A1%B5%E7%B2%BE%E9%80%89%E5%88%97%E8%A1%A8?_app_id=hottopic&_size=10&_fetch=1&_fetch_incrs=1&_max_pos='+maxPos+'&_fetch_total=1&_select=like_start%2Cdislike_start%2Ctitle%2Ctag%2Cmedia_data%2Clist_info%2Ccontent%2Cavatar%2Cuser_name%2Cis_hot%2Chot_comment%2Ccomment_total%2Coriginal%2Ctv_duration';
      
      this.$http.get(url).then((response) => {
        var json = JSON.parse(response.body);
        var list = json.data ;
        for (var i = 0; i < list.length; i++) {
          var joke = list[i];
          joke.commentList = [];
          joke.showCommentList = false;
          if (!joke.title) {
            joke.title = joke.content;
          }
          try{
            var imgs = JSON.parse(joke.media_data) 
            var size = imgs[0].origin_img_url.resolution
            joke.needExpand = size.substr(size.indexOf('x') + 1) > 900
            joke.origin_img_url = imgs[0].origin_img_url.origin_pic_url;
          }catch(e){
            joke.origin_img_url = "";
            joke.needExpand = false;
          }
          if (joke.origin_img_url == ""){
              this.jokeList.push(joke);
          }else if(!this.urlSet.has(joke.origin_img_url) ){
            this.urlSet.add(joke.origin_img_url);
            this.jokeList.push(joke);
          }
        }
      }, (error) => {
        console.error(error);
      })
    },
    nextPage () {
        this.maxPos = this.jokeList[this.jokeList.length-1]._pos;
        this.getData(this.maxPos);
    },
    expandImage(index){
       this.jokeList[index].needExpand = false;
    },
    clickImg(index){
       var url = this.jokeList[index].origin_img_url;
       var json = JSON.stringify({uris:[url]});
       var openUrl = 'hydrogen://pub.hydrogen.android?action=open_img&data='+json;
       var ifr = document.createElement('iframe');
       console.log(openUrl)
       ifr.src = openUrl;
       ifr.style.display = 'none';
       document.body.appendChild(ifr);
       setTimeout(function() {
          document.body.removeChild(ifr);
       }, 2000);
    },
    checkLoadMore(){
      // 检测滑动到底部
    },
    showCommentList (joke) {
      var url = 'http://bookshelf.leanapp.cn/qiqu?id='+ joke._id;
        this.$http.get(url).then((response) => {
          var json = JSON.parse(response.body);
          var list = json.data.comments;
          for (var i = 0; i < list.length; i++) {
            var comment = list[i];
            joke.commentList.push(comment);
          }
        }, (error) => {
          console.error(error);
        })
      joke.showCommentList = true;
    },
    hideCommentList (joke) {
      joke.commentList.length = 0;
      joke.showCommentList = false;
    }
  },
}
</script>

<style>
  * {
    padding: 0;
    margin: 0;
    -webkit-user-select: none;/*禁用手机浏览器的用户选择功能 */
    -moz-user-select: none;
  }

  html{
    font-size:14px;
  }

  body {
    font-family: Helvetica, sans-serif;
    background-color: #e1e1e1;
  }

  ul {
    list-style: none;
  }



  .main {
    width: 600px;
    margin: 0 auto;
    padding-bottom: 40px;
  }



  .joke {
    background-color: #ffffff;
    overflow: hidden;
    margin: 20px 40px;
  }

  .avatar {
    margin: 1rem;
    width: 2rem;
    height: 2rem;
    -webkit-border-radius: 100%;
    -webkit-border-radius: 100%;
    border-radius: 100%;
    margin-right: 10px;
    background: url(http://image.uc.cn/s/uae/g/0q/product/default-avatar.png) no-repeat;
    background-size: 2rem 2rem;
  }

  .username {
    margin: 1rem 0;
    display: inline-block;
    font-size: 0.9rem;
    line-height: 2rem;
    color: #888;
    -webkit-box-flex: 1;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .joke>p {
    margin: 0 1rem 1rem;
    font-size: 1rem;
    color: #4e4e4e;
    line-height: 1.5rem;
    word-break: break-all;
  }

  .pic {
    position: relative;
    overflow: hidden;
  }

  .pic>img {
    width: 100%;
  }

  .divider {
    border-bottom: 1px solid #e1e1e1;
  }

  .expand {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 1;
    margin: 0;
    background: rgba(0, 0, 0, .5);
    padding: 1rem 0;
    color: #fff;
    text-align: center;
    cursor: pointer;
  }

  .btn-more {
    width: 94%;
    display: flex;
    height: 3em;
    margin: 0 auto;
    background: none;
    border: none;
    font-size: 1rem;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    transition: background 0.4s;
  }

  .btn-more:hover {
    background: #eeeeee;
  }

  @media screen and (max-width: 600px){
     .main{
       width: 100%;
     }
     .joke{
        margin: 1rem 1rem;
     }
  }
</style>
