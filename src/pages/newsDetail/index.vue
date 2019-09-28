<template>
    <div>
      <ul>
          <li v-for="(detail,index) in newsDetail" :key="index">
              <p> {{detail.title}} </p>
              <p>{{detail.author}}</p>
              <p>{{detail.summary}}</p>
        <div class="content"><wxParse :content="detail.content" /></div>
        <!-- <div v-html="detail.content"></div> -->
          </li>
      </ul>
    </div>
</template>

<script>
import store from "@/pages/counter/store.js";
import wxParse from 'mpvue-wxparse'
export default {
    data(){
        return {

        }
    },
    components:{
    wxParse
    },
    computed:{
        newsDetail(){
           return store.state.newsDetail
        }
    },
    onLoad:function(options){
        console.log(options) 
        var aid = options.aid
        this.getnewsDetail(aid)
        wx.showShareMenu({
  withShareTicket: true
})

    },
   methods:{
        getnewsDetail(aid){
           var api = "http://www.phonegap100.com/appapi.php";
      wx.request({
        url: api,
        data: {
             a: "getPortalArticle",
         aid
        },
        header: {
          "content-type": "application/json" // 默认值
        },
        success(res) {
          console.log(res.data.result);
          store.commit("getnewsDetail", res.data.result);
        }
      });
    }
    },
  
   

}
</script>

<style  scoped>
@import url("~mpvue-wxparse/src/wxParse.css");
.content{
    padding: 10px;
    line-height: 2;
}
.content img{
            max-width:90%;
            margin: 0 auto;
}
</style>