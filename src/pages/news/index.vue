<template>
  <div>
    <ul>
      <li v-for="(news ,index) in newsList" :key="index">
        <p class="content" @click="gotoDetail(news.aid)">
          <span class="index">{{index+1}}</span>
          <span class="title">{{news.title}}</span>
        </p>
        <!-- <p><img :src="news.pic" alt=""></p> -->
      </li>
    </ul>
  </div>
</template>

<script>
import store from "@/pages/counter/store.js";
export default {
  data() {
    return {};
  },
  computed: {
    newsList() {
      return store.state.newsList;
    }
  },
  methods: {
      gotoDetail(aid){
            wx.navigateTo({
                url:`../newsDetail/main?aid=${aid}`
            })
      },
    getNewsList() {
      var api = "http://www.phonegap100.com/appapi.php";
      wx.request({
        url: api,
        data: {
          a: "getPortalList",
          catid: 20,
          page: 1
        },
        header: {
          "content-type": "application/json" // 默认值
        },
        success(res) {
        //   console.log(res.data.result);
          store.commit("getNewsList", res.data.result);
        }
      });
    }
  },
  created() {
    this.getNewsList();
  },

  mounted() {}
};
</script>

<style  scoped>
.index {
  font-weight: bold;
  font-size: 40rpx;
  margin-right: 40rpx;
}
.title {
  font-size: 30rpx;
  padding-right: 10rpx;
 
}
.title:hover{
    color:red;
    /* text-decoration: block; */
}
.content{
 overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-bottom: 50rpx;
}
</style>