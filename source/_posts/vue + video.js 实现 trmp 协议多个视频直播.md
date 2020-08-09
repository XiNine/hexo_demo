---
layout: 'vue'
title: 'vue + video.js 实现 trmp 协议多个视频直播'
date: 2020-08-08 22:30:12
categories: Vue
tags: Vue
---

**一、npm包准备**
```js
cnpm i video.js -S
cnpm i videojs-flash -S
```


**二、main.js 中进行引入**
```js
import Video from "video.js";
import "videojs-flash";
import "video.js/dist/video-js.min.css";
Vue.prototype.$video = Video;
```

**三、组件中进行使用**
```js
<!-- 视频组件 html部分 -->
<div class="mp4-wrap" v-for="item in videoList" :key="item.id">
  <video
    :id="`myVideo${item.id}`"
    class="video-js vjs-big-play-centered"
    data-setup="{}"
  >
    <source :src="item.url" type="rtmp/flv" />
  </video>
</div>
```

```js
data(){
  return {
    videoList: [//trmp视频源数组
        {
          url: "rtmp://58.200.131.2:1935/livetv/hunantv",
          id: 2
        },
        {
          url:
            "rtmp://rtmp01open.ys7.com/openlive/f01018a141094b7fa138b9d0b856507b.hd",
          id: 4
        },
        {
          url: "rtmp://119.23.19.140:1935/live/openUrl/YaVoFjO",
          id: 1
        },
        {
          url: "rtmp://58.200.131.2:1935/livetv/gxtv",
          id: 9
        }
      ],
  }
}

mounted(){
  this.initVideo();
},

methods:{
    initVideo() {
      let videoArr = [];
      this.videoList.map((item,index) => {
       //把视频配置项 push 进新数组逐个播放
        videoArr.push(
            this.$video("myVideo" + item.id, {
            controls: true,//显示控件
            autoplay: true,//自动播放
            preload: "auto",//预加载
            // poster: "",//封面图
          })
        )
        videoArr[index].play();
      });
      //离开组件时销毁每个实例
      this.$once('hook:beforeDestroy',()=>{
        for(let i in this.videoList){
          videoArr[i].dispose();
        }
      })
    },
}

/*
  Author：XiNine
 下期：vue + vue-video-player实现 HSL 多视频直播（海康）
*/
```
**Tips：**
1.经过试验，海康威视的trmp流无法播放，具体原因还不知道
2.不过下期的文章，有可以现实海康直播流的方案。

**效果图**
![vue+video.js播放trmp协议直播.png](/img/video-img.png)


