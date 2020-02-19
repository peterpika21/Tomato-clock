<template lang="pug">
  div.d-flex(style="height:100%")
    // 左側
    section.bg-gray(style="width:80px;height:100%")

    // 右側
    section.flex-1.bg-primary(style="height:100%")
      div.mx-auto.h-container.relative(style="height:100%")
        div.clock {{minutes}}:{{seconds}}
        div.d-flex.align-items-center
          div.p-2.border-s.border-white.round-full.border-solid
            embed(src="~assets/img/icon-bell.svg")
          div.p-3.border-s.border-white.round-full.border-solid
            embed(src="~assets/img/icon-play--orange.svg")
          div.p-2.border-s.border-white.round-full.border-solid
            embed(src="~assets/img/icon-delete.svg")

        // 底下番茄圖
        embed.svg--bottomTomato(src="~assets/img/tomato--orange.svg")
</template>

<script>
export default {
  data() {
    return {
      setSecond: 1500, // 初始共幾秒,
      minutes: '25', // 字幕顯示(分)
      seconds: '00' // 字幕顯示(秒)
    }
  },
  created() {
    let vm = this
    let demo = window.setInterval(function() {
      vm.counter()
    }, 1000)

    setTimeout(() => {
      window.clearInterval(demo)
    }, 5000)
  },
  methods: {
    counter() {
      let min = Math.floor(Math.floor(this.setSecond % 3600) / 60)
      let sec = this.setSecond % 60
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
    }
  }
}
</script>
