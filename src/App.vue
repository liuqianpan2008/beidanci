<template>
  <el-container>
    <el-header>
      <h1>背单词</h1>
    </el-header>
    <el-main>
      <div class="main">
        <span v-show="!play">任意键开始背单词</span>
        <div v-show="play">
          <div><span class="wordi"
                  v-for="(item, index) in world.world"
                  :key="index">{{index<=worldi-1? item : "_ "}}</span></div>
          <el-divider />
          <div>
            {{world.hanyi}}
            <el-button :icon="VideoPlay"
                       circle
                       @click="baofang(world.world)" />
          </div>
        </div>
      </div>
    </el-main>
    <el-footer>
      <div class="tjfoot">
        <el-card class="f-box">
          <div class="">
            <el-row :gutter="20">
              <el-col :span="5">
                <b>{{fg1.time}}</b>
                <el-divider />
                时间(s)
              </el-col>
              <el-col :span="5">
                <b>{{fg1.imp}}</b>
                <el-divider />
                输入
              </el-col>
              <el-col :span="5">
                <b>{{ Math.round(fg1.imp / (fg1.time == 0? 1 : fg1.time))+"个/s"}}</b>
                <el-divider />
                速度
              </el-col>
              <el-col :span="5">
                <b>{{fg1.zhengque}}</b>
                <el-divider />
                正确数
              </el-col>
              <el-col :span="4">
                <b>{{ Math.round(fg1.zhengque / (fg1.imp == 0? 1 : fg1.imp) * 10000) / 100 + "%"}}</b>
                <el-divider />
                正确率
              </el-col>
            </el-row>
          </div>
        </el-card>
      </div>
    </el-footer>
  </el-container>
</template>

<script>
import { ref } from '@vue/reactivity';
import data from './data.ts'
//video-play
import { VideoPlay } from '@element-plus/icons-vue'

export default {
  name: 'App',
  components: {
  },
  setup () {
    const fg = data;
    const world = ref({
      world: "one",
      hanyi: "1",
    })
    const fg1 = ref({
      time: 0,
      imp: 0,
      sudu: 0,
      zhengque: 0,
      zhengquelv: 0
    })
    const play = ref(false)
    const isworld = ref(true)
    var keycode = require('keycode');
    const worldi = ref(0);
    var myVar
    document.addEventListener('keydown', function (e) {
      if (play.value) {
        fg1.value.imp++
      }
      play.value = true;
      // 开始计时
      if (myVar == null) {
        myVar = setInterval(function () { fg1.value.time++; }, 1000);
      }
      //取随机单词
      if (isworld.value) {
        var index = Math.floor((Math.random() * data.length))
        world.value = data[index]
        console.log(world.value);
      }
      isworld.value = false;
      // 判断单词

      if (world.value.world[worldi.value] == keycode(e)) {
        console.log("正确");
        worldi.value++;
        fg1.value.zhengque++
        if (worldi.value == world.value.world.length) {
          console.log("全部正确");
          var index1 = Math.floor((Math.random() * data.length))
          world.value = data[index1]
          console.log(world.value);
          worldi.value = 0;
        }
      } else {
        console.log("错误");
        worldi.value = 0;

      }
    })
    const baofang = (world) => {
      let audio = new Audio()
      audio.src = "http://dict.youdao.com/dictvoice?type=0&audio=" + world
      audio.play()
    }
    return { play, fg, fg1, world, worldi, VideoPlay, baofang }
  }
}
</script>

<style>
.tjfoot {
  text-align: center;
  display: flex;
  justify-content: center;
}
.f-box {
  width: 60%;
}
.main {
  text-align: center;
  min-height: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.wordi {
  font: 3em sans-serif;
}
</style>
