<template>
  <div>
    <progress :percent="percent" color="green" v-if="show" />
    <swiper
      style="{width:'100%'}"
      autoplay="true"
      :interval="interval"
      :duration="duration"
      circular
      vertical
      v-else
    >
      <block v-for="(b,index) in banner" :key="index">
        <swiper-item style="{width:'100%'}">
          <img style="{width:'100%'}" :src="b.images.large" alt />
        </swiper-item>
      </block>
    </swiper>
    <map
      id="map"
      show-location='true'
      show-compass='true'
      :marker="marker"
      :longitude="longitude"
      :latitude="latitude"
      scale="14"
      style="width: 100%; height: 300px;"
    ></map>
  </div>
</template>

<script>
export default {
  data() {
    return {
      show: true,
      indicatorDots: true,
      vertical: true,
      circular: true,
      autoplay: false,
      interval: 2000,
      duration: 500,
      banner: [],
      percent: 0,
      longitude: "",
      latitude: "",
       markers: [{
     
      id: 0,
      latitude: 23.099994,
      longitude: 113.324520,
      width: 50,
      height: 50
    }],
      
    };
  },
  onLoad: function() {
    this.getlocation();
    var timer;

    timer = setInterval(() => {
      if (this.percent > 100) {
        console.log(999);

        this.show = false;
        clearInterval(timer);

        this.getBanner();
      }
      this.percent = ++this.percent;
    }, 10);
  },
  methods: {
    getBanner() {
      wx.request({
        url: "https://peng47.com:2906/vue/movie",
        data: {
          limit: 6
        },
        success: res => {
          this.banner = res.data.result;
        }
      });
    },
    getlocation() {
      wx.getLocation({
        type: "gcj02", //返回可以用于wx.openLocation的经纬度
        success:(res)=> {
          const latitude = res.latitude;
          const longitude = res.longitude;
           this.latitude = latitude;
            this.longitude = longitude;
        //   wx.openLocation({
        //     latitude,
        //     longitude,
        //     scale: 18,
        //      success:()=>{
        //     this.latitude = latitude;
        //     this.longitude = longitude;
        //   }
        //   },
        //  );
        }
      });
    }
  }
};
</script>

<style  scoped>
</style>