<template>
  <div class="overlay" :style="{ height: open ? '100%' : '0%' }">
    <a href="#" class="closebtn" @click="closeNav">&times;</a>
    <div class="overlay-content">
       <ul>
         <li v-for="(comment, index) in cList">
           <div class="comment-item">
             <h2>{{comment.author}}</h2>
             <p>{{comment.message}}</p>
           </div>
         </li>
       </ul>
    </div>
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
    open: Boolean
  },

  data () {
    return {
      cList: [],
    }
  },
  created () {
    this.getData()
  },
  methods: {
    closeNav () {
        this.$parent.openCommentList = false
    },
    getData(){ 
      var url = 'http://bookshelf.leanapp.cn/qiqu?id='+ this.joke._id
      this.$http.get(url).then((response) => {
        var json = JSON.parse(response.body)
        var list = json.data.comments
        for (var i = 0; i < list.length; i++) {
          var comment = list[i]
          this.cList.push(comment)
        } 
      }, (error) => {
        console.error(error)
      })
    }
  }
}
</script>

<style>
.overlay {
    height: 0%;
    width: 100%;
    position: fixed;
    z-index: 1;
    top: 0;
    left: 0;
    color: white;
    background-color: rgb(0,0,0);
    background-color: rgba(0,0,0, 0.9);
    overflow-y: hidden;
    transition: 0.5s;
}

.overlay-content {
    position: relative;
    top: 25%;
    width: 100%;
    text-align: center;
    margin-top: 30px;
}

.overlay a {
    padding: 8px;
    text-decoration: none;
    font-size: 36px;
    color: #818181;
    display: block;
    transition: 0.3s;
}

.overlay a:hover, .overlay a:focus {
    color: #f1f1f1;
}

.overlay .closebtn {
    position: absolute;
    top: 20px;
    right: 45px;
    font-size: 60px;
}

@media screen and (max-height: 450px) {
  .overlay {overflow-y: auto;}
  .overlay a {font-size: 20px}
  .overlay .closebtn {
    font-size: 40px;
    top: 15px;
    right: 35px;
  }
}

.comment-item{
  width: 600px;
  display: flex;
  margin: 0 auto;
  flex-direction: column;
  align-items: flex-start;
  border-bottom: 1px dashed #888888;
  margin-bottom: 1em;
}
.comment-item>h2{
  color: #888888;
  font-size: 1em;
}
.comment-item>p{
  font-size: 1.2em;
  line-height: 1.2em;
  margin: 1em 0em;
}

</style>
