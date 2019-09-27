<template>
  <div @click="clickHandle">
    <div class="userinfo" @click="bindViewTap">
      <img
        class="userinfo-avatar"
        v-if="userInfo.avatarUrl"
        :src="userInfo.avatarUrl"
        background-size="cover"
      />
      <!-- <img class="userinfo-avatar" src="/static/images/user.png" background-size="cover" /> -->

      <div class="userinfo-nickname">
        <card :text="userInfo.nickName"></card>
      </div>
    </div>
  

    <!-- <div class="usermotto">
      <div class="user-motto">
        <card :text="motto"></card>
      </div>
    </div>-->

    <!-- <form class="form-container">
      <input type="text" class="form-control" :value="motto" placeholder="v-model" />
      <input type="text" class="form-control" v-model="motto" placeholder="v-model" />
      <input type="text" class="form-control" v-model.lazy="motto" placeholder="v-model.lazy" />
    </form>-->

    <!-- <a href="/pages/counter/main" class="counter">去往Vuex示例页面</a> -->

    <!-- <div class="all">
        <div class="left">
        </div>
        <div class="right">
        </div>
    
    </div>-->
  </div>
</template>

<script>
import card from "@/components/card";

export default {
  data() {
    return {
      userInfo: {
        nickName: "",
        avatarUrl: ""
      }
    };
  },

  components: {
    card
  },

  methods: {
    // bindViewTap () {
    //   const url = '../home/main'
    //   if (mpvuePlatform === 'wx') {
    //     mpvue.switchTab({ url })
    //   } else {
    //     mpvue.navigateTo({ url })
    //   }
    // },
    clickHandle(ev) {
      console.log("clickHandle:", ev);
      // throw {message: 'custom test'}
    },
    //是否授权
    getAuth() {
      // 可以通过 wx.getSetting 先查询一下用户是否授权了 "scope.record" 这个 scope
      wx.getSetting({
        success(res) {
          
          if (!res.authSetting["scope.record"]) {
            wx.authorize({
              scope: "scope.record",
              success() {
                
                // 用户已经同意小程序使用录音功能，后续调用 wx.startRecord 接口不会弹窗询问
                wx.startRecord();
              }
            });
          }
          const url = "../home/main";
                if (mpvuePlatform === "wx") {
                  mpvue.switchTab({ url });
                } else {
                  mpvue.navigateTo({ url });
                }
        }
      });
    }
  },

  created() {
    // let app = getApp()
  },
  onLoad: function(options) {
    this.getAuth()
    var that = this;

    // 查看是否授权
    wx.getSetting({
      success(res) {
        if (res.authSetting["scope.userInfo"]) {
          // 已经授权，可以直接调用 getUserInfo 获取头像昵称
          wx.getUserInfo({
            success: res => {
              console.log(res.userInfo);
              that.userInfo.nickName = res.userInfo.nickName;
              that.userInfo.avatarUrl = res.userInfo.avatarUrl;
            }
          });
        }
      }
    });
  }
};
</script>

<style scoped>
.userinfo {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.userinfo-avatar {
  width: 128rpx;
  height: 128rpx;
  margin: 20rpx;
  border-radius: 50%;
}

.userinfo-nickname {
  color: #aaa;
}

.usermotto {
  margin-top: 150px;
}

.form-control {
  display: block;
  padding: 0 12px;
  margin-bottom: 5px;
  border: 1px solid #ccc;
}
.all {
  width: 7.5rem;
  height: 1rem;
  background-color: blue;
}
.all:after {
  display: block;
  content: "";
  clear: both;
}
.left {
  float: left;
  width: 3rem;
  height: 1rem;
  background-color: red;
}

.right {
  float: left;
  width: 4.5rem;
  height: 1rem;
  background-color: green;
}
</style>
