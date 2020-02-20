<template lang="pug">
  section.flex-1.bg-primary(style="height:100%")
    div.pt-6.mx-auto.h-container.relative(style="height:100%")
      p.tx-gray.align-right {{fullDate}}

      div.d-flex.justify-content-between
        // 左側
        div
          div.clock {{minutes}}:{{seconds}}
          div.d-flex.align-items-center(style="margin-left:137px")
            div.mr-4.p-2.svg--border(:class="{'bg-success':!$refs.audio.muted,'border-success':!$refs.audio.muted}",@click="$refs.audio.muted = !$refs.audio.muted")
              img(src="~assets/img/icon-bell.svg")
            div.mr-4.p-4.svg--border.bg-gray(@click="start()")
              img(v-if="isStart",src=`~assets/img/icon-pause--orange.svg`)
              img(v-else,src=`~assets/img/icon-play--orange.svg`)
            div.p-2.svg--border(@click="clear()")
              img(src="~assets/img/icon-delete.svg")

        div.mt-6
          p.tx-gray.font-style2.font-xxl(v-if="isStart") 奮鬥，GO GO GO !!
          p.tx-gray.font-style2.font-xxl(v-else) 休息，能走更長遠的路

      // 底下番茄圖
      img.svg--bottomTomato(v-if="isStart",src="~assets/img/tomato--orange.svg")
      img.svg--bottomTomato(v-else,src="~assets/img/tomato--green.svg")

      audio(ref="audio" src="~assets/music/01.mp3" type="audio/mpeg" loop)
</template>

<script>
export default {
  data() {
    return {
      isStart: false,
      setSecond: 1500, // 初始共幾秒,
      minutes: '25', // 字幕顯示(分)
      seconds: '00', // 字幕顯示(秒)
      date: new Date(),
      interval: ''
    }
  },
  computed: {
    fullDate() {
      let year = this.date.getFullYear()
      let month = this.date.getMonth() + 1
      let day = this.date.getDate()
      let hour = this.date.getHours()
      let min = this.date.getMinutes()

      let str = `${year}.${month}.${day}  ${hour < 10 ? '0' : ''}${hour}:${min < 10 ? '0' : ''}${min}`
      return str
    }
  },
  created() {
    let vm = this
    setInterval(() => {
      vm.date = new Date()
    }, 1000)
  },
  methods: {
    start() {
      let vm = this
      if (this.isStart) {
        window.clearInterval(this.interval)
        this.$refs.audio.pause()
      } else {
        this.interval = window.setInterval(function() {
          vm.counter()
        }, 1000)
        this.$refs.audio.play()
      }
      this.isStart = !this.isStart
    },
    clear() {
      this.$refs.audio.load()
      window.clearInterval(this.interval)
      this.setSecond = 1500
      this.minutes = '25'
      this.seconds = '00'
      this.isStart = false
    },
    // 計算時間
    counter() {
      let min = Math.floor(Math.floor(this.setSecond % 3600) / 60)
      let sec = this.setSecond % 60
      if (this.setSecond > 0) {
        this.setSecond -= 1

        // 分數格式調整
        if (min < 10) {
          this.minutes = '0' + min
        } else {
          this.minutes = min
        }

        // 秒數格式調整
        if (sec < 10) {
          this.seconds = '0' + sec
        } else {
          this.seconds = sec
        }
      } else {
        this.clear()
      }
    }
  }
}
</script>
