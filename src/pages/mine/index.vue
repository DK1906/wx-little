<template>
  <div>
    <!-- <view class="section tc">
  <video src="{{src}}"   controls ></video>
  <view class="btn-area">
    <button @bindtap="bindButtonTap">获取视频</button>
  </view>
    </view>-->

    <view class="section tc">
      <video
        id="myVideo"
        src="http://wxsnsdy.tc.qq.com/105/20210/snsdyvideodownload?filekey=30280201010421301f0201690402534804102ca905ce620b1241b726bc41dcff44e00204012882540400&bizid=1023&hy=SH&fileparam=302c020101042530230204136ffd93020457e3c4ff02024ef202031e8d7f02030f42400204045a320a0201000400"
        :danmu-list="danmuList"
        enable-danmu
        danmu-btn
        controls
        @play="bindplay"
       @timeupdate='bindtimeupdate'
      ></video>
      <view class="btn-area">
        <!-- <button @click="bindButtonTap">获取视频</button> -->
        <input @input="bindInputBlur"  class="danmuContent"  v-model="inputValue"/>
        <button @click="bindSendDanmu">发送弹幕</button>
      </view>
    </view>
  </div>
</template>

<script>
export default {
  data() {
    return {
      danmuList: [
      {
        text: '第 1s 出现的弹幕',
        color: '#ff0000',
        time: 1
      },
      {
        text: '第 3s 出现的弹幕',
        color: '#ff00ff',
        time: 3
    }],
    inputValue:'',
    sendDanmu:'',
    videoContext:'',
    value:"",
    networkType:''
    };
  },
   onReady: function (res) {
    this.videoContext = wx.createVideoContext('myVideo')
  },
  onLoad:()=>{
      //获取网络状态
      wx.getNetworkType({
  success:(res)=> {
    this.networkType = res.networkType
  }
})
//监听网路状态变化
wx.onNetworkStatusChange((res)=> {
  console.log(res.isConnected)
  console.log(res.networkType)
})

  },
  methods: {
      bindplay(){
            //开始播放时的事件
            console.log('bindplay')
      },
      //播放过程中的时间
      bindtimeupdate(){
 console.log('bindtimeupdate')
      },
    getRandomColor() {
      let rgb = [];
      for (let i = 0; i < 3; ++i) {
        let color = Math.floor(Math.random() * 256).toString(16);
        color = color.length == 1 ? "0" + color : color;
        rgb.push(color);
      }
      return "#" + rgb.join("");
    },
     bindInputBlur(e) {
       
    this.inputValue = e.target.value;
 
     console.log( e.target.value)
    
  
  },
//   bindButtonTap() {
//     var that = this
//     wx.chooseVideo({
//       sourceType: ['album', 'camera'],
//       maxDuration: 60,
//       camera: ['front','back'],
//       success:(res)=> {
//         that.setData({
//           src: res.tempFilePath
//         })
//       }
//     })
//   },

  bindSendDanmu () {
  
        this.videoContext.sendDanmu({
      text: this.inputValue,
      color: this.getRandomColor(),
   
       } )
  
    
      
  },


  }
};
</script>