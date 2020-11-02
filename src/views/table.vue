<template>
  <div class="full">
    <div ref="table" id="table" class="table">
      <div
        ref="view"
        class="view">
      </div>
    </div>
    <div>
      <button>开始游戏</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Table',

  data () {
    return {
      sharpper: []
    }
  },

  mounted () {
    this.start()
  },

  methods: {
    start () {
      this.create()
      const sharpper = this.sharpper
      const sharp = sharpper[0]
      const type = sharpper[1]
      const round = this.downRound(sharp, type)
      this.keydown()
      // 获取新方块周围
      let ifEnd
      for (let index = 0; index < round.length; index++) {
        if (round[index] !== 'view') {
          ifEnd = true
        }
      }
      // 判断新方块是否溢出，如果溢出gameover，反之goon
      if (ifEnd) {
        alert('game over')
      } else {
        const fall = setInterval(() => {
          let round = this.downRound(sharp, type)
          // 获取当前方块周围
          let ifmove = ''
          for (let index = 0; index < round.length; index++) {
            if (round[index] !== 'view') {
              ifmove = 'stop'
            }
          }
          // 判断当前方块是否到底，如果到底停止，反之坠落
          if (ifmove !== 'stop') {
            this.down()
            round = []
          } else {
            ifmove = 0
            clearInterval(fall)
            this.clearLastRow()
            this.start()
          }
        }, 250)
      }
    },
    // 创建方块
    create () {
      const view = this.$refs.view
      let sharp = ''
      var type = Math.floor(Math.random() * 6)
      type = 0 // test
      sharp = document.createElement('div')
      sharp.classList.add('sharp')
      switch (type) {
        case 0:
          sharp.classList.add('O')
          break
        case 1:
          sharp.classList.add('I')
          break
        case 2:
          sharp.classList.add('Z')
          break
        case 3:
          sharp.classList.add('S')
          break
        case 4:
          sharp.classList.add('J')
          break
        case 5:
          sharp.classList.add('L')
          break
        case 6:
          sharp.classList.add('T')
          break
      }
      // type = 6 // ////////////////////////
      view.appendChild(sharp)
      sharp.style.top = '0px'
      this.sharpper = [sharp, type]
    },
    // 清楚最后一行
    clearLastRow () {
      let rows = 0
      for (let index = 0; index < 4; index++) {
        for (let i = 0; i < 10; i++) {
          if (document.elementFromPoint(52.5 + 30 * i, 585 - index * 30).className !== 'view') {
            rows++
          }
        }
        if (rows === 10) {
          this.$refs.view.style.height = this.$refs.view.clientHeight + 30 + 'px'
          document.getElementsByClassName('sharp').forEach(element => {
            element.style.top = element.offsetTop + 30 + 'px'
          })
        }
      }
    },
    // 设置下周围元素
    downRound () {
      const sharpper = this.sharpper
      const sharp = sharpper[0]
      const type = sharpper[1]
      const left = sharp.offsetLeft + 37.5
      const top = sharp.offsetTop
      const round = []
      switch (type) {
        case 0:
          // 设置O形周围
          round.push(document.elementFromPoint(left + 15, top + 75).className)
          round.push(document.elementFromPoint(left + 45, top + 75).className)
          break

        case 1:
          // 设置I形周围
          round.push(document.elementFromPoint(left, top + 135).className)
          break
        case 1090:
          // 设置I形周围
          round.push(document.elementFromPoint(left - 15, top + 45).className)
          round.push(document.elementFromPoint(left - 45, top + 45).className)
          round.push(document.elementFromPoint(left - 75, top + 45).className)
          round.push(document.elementFromPoint(left - 105, top + 45).className)
          break

        case 2:
          // 设置Z形周围
          round.push(document.elementFromPoint(left + 15, top + 45).className)
          round.push(document.elementFromPoint(left + 45, top + 75).className)
          round.push(document.elementFromPoint(left + 75, top + 75).className)
          break
        case 2090:
          // 设置Z90形周围
          round.push(document.elementFromPoint(left - 15, top + 75).className)
          round.push(document.elementFromPoint(left - 45, top + 105).className)
          break

        case 3:
          // 设置S形周围
          round.push(document.elementFromPoint(left - 15, top + 75).className)
          round.push(document.elementFromPoint(left + 15, top + 75).className)
          round.push(document.elementFromPoint(left + 45, top + 45).className)
          break
        case 3090:
          // 设置S形周围
          round.push(document.elementFromPoint(left - 45, top + 45).className)
          round.push(document.elementFromPoint(left - 15, top + 75).className)
          break

        case 4:
          // 设置J形周围
          round.push(document.elementFromPoint(left - 15, top + 105).className)
          round.push(document.elementFromPoint(left + 15, top + 105).className)
          break
        case 4090:
          // 设置J90形周围
          round.push(document.elementFromPoint(left - 15, top + 45).className)
          round.push(document.elementFromPoint(left - 45, top + 45).className)
          round.push(document.elementFromPoint(left - 75, top + 45).className)
          break
        case 4180:
          // 设置 J180 形周围 写到这里！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left + 15, top - 45).className)
          break
        case 4270:
          // 设置 J270 形周围 写到这里！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！
          round.push(document.elementFromPoint(left + 15, top + 15).className)
          round.push(document.elementFromPoint(left + 45, top + 15).className)
          round.push(document.elementFromPoint(left + 75, top + 45).className)
          break

        case 5:
          // 设置 L 形周围
          round.push(document.elementFromPoint(left + 15, top + 105).className)
          round.push(document.elementFromPoint(left + 45, top + 105).className)
          break
        case 5090:
          // 设置 L90 形周围
          round.push(document.elementFromPoint(left - 15, top + 45).className)
          round.push(document.elementFromPoint(left - 45, top + 45).className)
          round.push(document.elementFromPoint(left - 75, top + 75).className)
          break
        case 5180:
          // 设置 L180 形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left - 45, top - 45).className)
          break
        case 5270:
          // 设置 L270 形周围
          round.push(document.elementFromPoint(left + 15, top + 15).className)
          round.push(document.elementFromPoint(left + 45, top + 15).className)
          round.push(document.elementFromPoint(left + 75, top + 15).className)
          break

        case 6:
          // 设置T形周围
          round.push(document.elementFromPoint(left + 15, top + 45).className)
          round.push(document.elementFromPoint(left + 45, top + 45).className)
          round.push(document.elementFromPoint(left + 75, top + 45).className)
          break
        case 6090:
          // 设置 T90 形周围
          round.push(document.elementFromPoint(left - 15, top + 105).className)
          round.push(document.elementFromPoint(left + 15, top + 75).className)
          break
        case 6180:
          // 设置 T180 形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left - 45, top + 45).className)
          round.push(document.elementFromPoint(left - 75, top + 15).className)
          break
        case 6270:
          // 设置 T270 形周围
          round.push(document.elementFromPoint(left - 15, top - 15).className)
          round.push(document.elementFromPoint(left + 15, top + 15).className)
          break
      }
      return round
    },
    // 设置左周围元素
    leftRound () {
      const sharpper = this.sharpper
      const sharp = sharpper[0]
      const type = sharpper[1]
      const left = sharp.offsetLeft + 37.5
      const top = sharp.offsetTop
      const round = []

      switch (type) {
        case 0:
          // 设置O形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left - 15, top + 45).className)
          break

        case 1:
          // 设置I形周围
          round.push(document.elementFromPoint(left - 15, sharp.offsetTop + 15).className)
          round.push(document.elementFromPoint(left - 15, sharp.offsetTop + 45).className)
          round.push(document.elementFromPoint(left - 15, sharp.offsetTop + 75).className)
          round.push(document.elementFromPoint(left - 15, sharp.offsetTop + 105).className)
          break
        case 1090:
          // 设置I90形周围
          round.push(document.elementFromPoint(left - 135, top + 15).className)
          break

        case 2:
          // 设置Z形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left + 15, top + 45).className)
          break
        case 2090:
          // 设置Z90形周围
          round.push(document.elementFromPoint(left - 45, top + 15).className)
          round.push(document.elementFromPoint(left - 75, top + 45).className)
          round.push(document.elementFromPoint(left - 75, top + 75).className)
          break

        case 3:
          // 设置S形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left - 45, top + 45).className)
          break
        case 3090:
          // 设置S形周围
          round.push(document.elementFromPoint(left - 75, top - 15).className)
          round.push(document.elementFromPoint(left - 75, top + 15).className)
          round.push(document.elementFromPoint(left - 45, top + 45).className)
          break

        case 4:
          // 设置J形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left - 15, top + 45).className)
          round.push(document.elementFromPoint(left - 45, top + 75).className)
          break
        case 4090:
          // 设置J90形周围
          round.push(document.elementFromPoint(left - 105, top - 15).className)
          round.push(document.elementFromPoint(left - 105, top + 15).className)
          break
        case 4180:
          // 设置J180形周围
          round.push(document.elementFromPoint(left - 45, top - 15).className)
          round.push(document.elementFromPoint(left - 45, top - 45).className)
          round.push(document.elementFromPoint(left - 45, top - 75).className)
          break
        case 4270:
          // 设置 J270 形周围 写到这里！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！
          round.push(document.elementFromPoint(left - 15, top - 15).className)
          round.push(document.elementFromPoint(left + 45, top + 15).className)
          break

        case 5:
          // 设置 L 形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left - 15, top + 45).className)
          round.push(document.elementFromPoint(left - 15, top + 75).className)
          break
        case 5090:
          // 设置 L90形周围
          round.push(document.elementFromPoint(left - 105, top + 45).className)
          round.push(document.elementFromPoint(left - 105, top + 15).className)
          break
        case 5180:
          // 设置 L180 形周围
          round.push(document.elementFromPoint(left - 45, top - 15).className)
          round.push(document.elementFromPoint(left - 45, top - 45).className)
          round.push(document.elementFromPoint(left - 75, top - 75).className)
          break
        case 5270:
          // 设置 L270 形周围
          round.push(document.elementFromPoint(left - 15, top - 15).className)
          round.push(document.elementFromPoint(left + 45, top - 45).className)
          break

        case 6:
          // 设置T形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left + 15, top - 15).className)
          break
        case 6090:
          // 设置 T90 形周围
          round.push(document.elementFromPoint(left - 45, top + 15).className)
          round.push(document.elementFromPoint(left - 45, top + 45).className)
          round.push(document.elementFromPoint(left - 45, top + 75).className)
          break
        case 6180:
          // 设置 T180 形周围
          round.push(document.elementFromPoint(left - 75, top + 15).className)
          round.push(document.elementFromPoint(left - 105, top - 15).className)
          break
        case 6270:
          // 设置 T270 形周围
          round.push(document.elementFromPoint(left - 15, top - 15).className)
          round.push(document.elementFromPoint(left - 45, top - 45).className)
          round.push(document.elementFromPoint(left - 15, top - 75).className)
          break
      }
      return round
    },
    // 设置右周围元素
    rightRound () {
      const sharpper = this.sharpper
      const sharp = sharpper[0]
      const type = sharpper[1]
      const left = sharp.offsetLeft + 37.5
      const top = sharp.offsetTop
      const round = []

      switch (type) {
        case 0:
          // 设置O形周围
          round.push(document.elementFromPoint(left + 60 + 15, top + 15).className)
          round.push(document.elementFromPoint(left + 60 + 15, top + 45).className)
          break

        case 1:
          // 设置I形周围
          round.push(document.elementFromPoint(left + 45, sharp.offsetTop + 15).className)
          round.push(document.elementFromPoint(left + 45, sharp.offsetTop + 45).className)
          round.push(document.elementFromPoint(left + 45, sharp.offsetTop + 75).className)
          round.push(document.elementFromPoint(left + 45, sharp.offsetTop + 105).className)
          break
        case 1090:
          // 设置I90形周围
          round.push(document.elementFromPoint(left + 15, top + 15).className)
          break

        case 2:
          // 设置Z形周围
          round.push(document.elementFromPoint(left + 75, top + 15).className)
          round.push(document.elementFromPoint(left + 105, top + 45).className)
          break
        case 2090:
          // 设置Z90形周围
          round.push(document.elementFromPoint(left + 15, top + 15).className)
          round.push(document.elementFromPoint(left + 15, top + 45).className)
          round.push(document.elementFromPoint(left - 15, top + 75).className)
          break

        case 3:
          // 设置S形周围
          round.push(document.elementFromPoint(left + 75, top + 15).className)
          round.push(document.elementFromPoint(left + 45, top + 45).className)
          break
        case 3090:
          // 设置S形周围
          round.push(document.elementFromPoint(left - 15, top - 15).className)
          round.push(document.elementFromPoint(left + 15, top + 15).className)
          round.push(document.elementFromPoint(left + 15, top + 45).className)
          break

        case 4:
          // 设置J形周围
          round.push(document.elementFromPoint(left + 45, top + 15).className)
          round.push(document.elementFromPoint(left + 45, top + 45).className)
          round.push(document.elementFromPoint(left + 45, top + 75).className)
          break
        case 4090:
          // 设置J90形周围
          round.push(document.elementFromPoint(left - 45, top - 15).className)
          round.push(document.elementFromPoint(left + 15, top + 15).className)
          break
        case 4180:
          // 设置J180形周围
          round.push(document.elementFromPoint(left + 15, top - 15).className)
          round.push(document.elementFromPoint(left + 15, top - 45).className)
          round.push(document.elementFromPoint(left + 45, top - 75).className)
          break
        case 4270:
          // 设置 J270 形周围
          round.push(document.elementFromPoint(left + 105, top - 15).className)
          round.push(document.elementFromPoint(left + 105, top + 15).className)
          break

        case 5:
          // 设置 L 形周围
          round.push(document.elementFromPoint(left + 45, top + 15).className)
          round.push(document.elementFromPoint(left + 45, top + 45).className)
          round.push(document.elementFromPoint(left + 75, top + 75).className)
          break
        case 5090:
          // 设置 L90 形周围
          round.push(document.elementFromPoint(left - 45, top + 45).className)
          round.push(document.elementFromPoint(left + 15, top + 15).className)
          break
        case 5180:
          // 设置 L180 形周围
          round.push(document.elementFromPoint(left + 15, top - 15).className)
          round.push(document.elementFromPoint(left + 15, top - 45).className)
          round.push(document.elementFromPoint(left + 15, top - 75).className)
          break
        case 5270:
          // 设置 L270 形周围
          round.push(document.elementFromPoint(left + 105, top - 15).className)
          round.push(document.elementFromPoint(left + 105, top - 45).className)
          break

        case 6:
          // 设置T形周围
          round.push(document.elementFromPoint(left + 75, top - 15).className)
          round.push(document.elementFromPoint(left + 105, top + 15).className)
          break
        case 6090:
          // 设置 T90 形周围
          round.push(document.elementFromPoint(left + 15, top + 15).className)
          round.push(document.elementFromPoint(left + 45, top + 45).className)
          round.push(document.elementFromPoint(left + 15, top + 75).className)
          break
        case 6180:
          // 设置 T180 形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left + 15, top - 15).className)
          break
        case 6270:
          // 设置 T270 形周围
          round.push(document.elementFromPoint(left + 45, top - 15).className)
          round.push(document.elementFromPoint(left + 45, top - 45).className)
          round.push(document.elementFromPoint(left + 45, top - 75).className)
          break
      }
      return round
    },
    // 设置旋转占位元素
    rotateRound () {
      const sharpper = this.sharpper
      const sharp = sharpper[0]
      const type = sharpper[1]
      const left = sharp.offsetLeft + 37.5
      const top = sharp.offsetTop
      const round = []

      switch (type) {
        case 0:
          // 设置O形周围
          break
        case 1:
          // 设置I形周围
          round.push(document.elementFromPoint(left - 15, sharp.offsetTop + 15).className)
          round.push(document.elementFromPoint(left - 45, sharp.offsetTop + 15).className)
          round.push(document.elementFromPoint(left - 75, sharp.offsetTop + 15).className)
          round.push(document.elementFromPoint(left - 105, sharp.offsetTop + 15).className)
          break
        case 1090:
          // 设置I形周围
          round.push(document.elementFromPoint(left + 15, sharp.offsetTop + 15).className)
          round.push(document.elementFromPoint(left + 15, sharp.offsetTop + 45).className)
          round.push(document.elementFromPoint(left + 15, sharp.offsetTop + 75).className)
          round.push(document.elementFromPoint(left + 15, sharp.offsetTop + 105).className)
          break
        case 2:
          // 设置Z形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left - 15, top + 45).className)
          round.push(document.elementFromPoint(left - 45, top + 75).className)
          round.push(document.elementFromPoint(left - 45, top + 105).className)
          break
        case 2090:
          // 设置Z90形周围
          round.push(document.elementFromPoint(left + 15, top + 15).className)
          round.push(document.elementFromPoint(left + 45, top + 15).className)
          round.push(document.elementFromPoint(left + 45, top + 45).className)
          round.push(document.elementFromPoint(left + 75, top + 45).className)
          break
        case 3:
          // 设置S形周围
          round.push(document.elementFromPoint(left - 45, top - 15).className)
          round.push(document.elementFromPoint(left - 45, top + 15).className)
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          break
        case 3090:
          // 设置S形周围
          round.push(document.elementFromPoint(left + 45, top + 15).className)
          round.push(document.elementFromPoint(left + 15, top + 15).className)
          round.push(document.elementFromPoint(left + 15, top + 45).className)
          break
        case 4:
          // 设置J形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left - 45, top + 15).className)
          round.push(document.elementFromPoint(left - 75, top + 15).className)
          round.push(document.elementFromPoint(left - 75, top - 15).className)
          break
        case 4090:
          // 设置J90形周围
          round.push(document.elementFromPoint(left - 15, top - 15).className)
          round.push(document.elementFromPoint(left - 15, top - 45).className)
          round.push(document.elementFromPoint(left - 15, top - 75).className)
          round.push(document.elementFromPoint(left + 15, top - 75).className)
          break
        case 4180:
          // 设置J180形周围
          round.push(document.elementFromPoint(left + 15, top - 15).className)
          round.push(document.elementFromPoint(left + 45, top - 15).className)
          round.push(document.elementFromPoint(left + 75, top - 15).className)
          round.push(document.elementFromPoint(left + 75, top + 15).className)
          break
        case 4270:
          // 设置 J270 形周围 写到这里！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！
          round.push(document.elementFromPoint(left + 15, top + 15).className)
          round.push(document.elementFromPoint(left + 15, top + 45).className)
          round.push(document.elementFromPoint(left + 15, top + 75).className)
          round.push(document.elementFromPoint(left - 15, top + 75).className)
          break

        case 5:
          // 设置 L 形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left - 45, top + 15).className)
          round.push(document.elementFromPoint(left - 75, top + 15).className)
          round.push(document.elementFromPoint(left - 75, top + 45).className)
          break
        case 5090:
          // 设置 L90 形周围
          round.push(document.elementFromPoint(left - 15, top - 15).className)
          round.push(document.elementFromPoint(left - 45, top - 15).className)
          round.push(document.elementFromPoint(left - 75, top - 15).className)
          round.push(document.elementFromPoint(left - 75, top - 45).className)
          break
        case 5180:
          // 设置 L180 形周围
          round.push(document.elementFromPoint(left + 15, top - 15).className)
          round.push(document.elementFromPoint(left + 45, top - 15).className)
          round.push(document.elementFromPoint(left + 75, top - 15).className)
          round.push(document.elementFromPoint(left + 75, top - 45).className)
          break
        case 5270:
          // 设置 L270 形周围
          round.push(document.elementFromPoint(left + 15, top + 15).className)
          round.push(document.elementFromPoint(left + 15, top + 45).className)
          round.push(document.elementFromPoint(left + 15, top + 75).className)
          round.push(document.elementFromPoint(left + 45, top + 75).className)
          break
        case 6:
          // 设置 T 形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left - 15, top + 45).className)
          round.push(document.elementFromPoint(left - 15, top + 75).className)
          round.push(document.elementFromPoint(left + 15, top + 45).className)
          break
        case 6090:
          // 设置 T90 形周围
          round.push(document.elementFromPoint(left - 15, top - 15).className)
          round.push(document.elementFromPoint(left - 45, top - 15).className)
          round.push(document.elementFromPoint(left - 45, top + 15).className)
          round.push(document.elementFromPoint(left - 75, top - 15).className)
          break
        case 6180:
          // 设置 T180 形周围
          round.push(document.elementFromPoint(left + 15, top - 15).className)
          round.push(document.elementFromPoint(left + 15, top - 45).className)
          round.push(document.elementFromPoint(left - 15, top - 45).className)
          round.push(document.elementFromPoint(left + 15, top - 75).className)
          break
        case 6270:
          // 设置 T270 形周围
          round.push(document.elementFromPoint(left + 15, top + 15).className)
          round.push(document.elementFromPoint(left + 45, top + 15).className)
          round.push(document.elementFromPoint(left + 45, top - 15).className)
          round.push(document.elementFromPoint(left + 75, top + 15).className)
          break
      }
      return round
    },
    // 监听键盘
    keydown () {
      const sharpper = this.sharpper
      const _this = this
      let ifR = true
      let round
      document.onkeydown = function (e) {
        const key = window.event.keyCode
        if (key === 38) { // 上方向键
          switch (sharpper[1]) {
            case 0:
              break
            case 1:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(90deg)'
                _this.sharpper[1] = 1090
              }
              break
            case 1090:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(0deg)'
                _this.sharpper[1] = 1
              }
              break
            case 2:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(90deg)'
                _this.sharpper[1] = 2090
              }
              break
            case 2090:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(0deg)'
                _this.sharpper[1] = 2
                ifR = true
              }
              break
            case 3:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(90deg)'
                _this.sharpper[1] = 3090
              }
              break
            case 3090:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(0deg)'
                _this.sharpper[1] = 3
              }
              break
            case 4:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(90deg)'
                _this.sharpper[1] = 4090
              }
              break
            case 4090:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(180deg)'
                _this.sharpper[1] = 4180
              }
              break
            case 4180:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(270deg)'
                _this.sharpper[1] = 4270
              }
              break
            case 4270:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = ''
                _this.sharpper[1] = 4
              }
              break
            case 5:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(90deg)'
                _this.sharpper[1] = 5090
              }
              break
            case 5090:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(180deg)'
                _this.sharpper[1] = 5180
              }
              break
            case 5180:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(270deg)'
                _this.sharpper[1] = 5270
              }
              break
            case 5270:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = ''
                _this.sharpper[1] = 5
              }
              break
            case 6:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(90deg)'
                _this.sharpper[1] = 6090
              }
              break
            case 6090:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(180deg)'
                _this.sharpper[1] = 6180
              }
              break
            case 6180:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = 'rotate(270deg)'
                _this.sharpper[1] = 6270
              }
              break
            case 6270:
              ifR = true
              round = _this.rotateRound(sharpper[0], sharpper[1])
              for (let index = 0; index < round.length; index++) {
                if (round[index] !== 'view') {
                  ifR = false
                }
              }
              if (ifR) {
                sharpper[0].style.transform = ''
                _this.sharpper[1] = 6
              }
              break
          }
        } else if (key === 37) { // 左方向键
          const round = _this.leftRound(sharpper[0], sharpper[1])
          let ifStop
          for (let index = 0; index < round.length; index++) {
            if (round[index] !== 'view') {
              ifStop = true
            }
          }
          if (!ifStop) {
            _this.left(sharpper[0])
          }
        } else if (key === 39) { // 右方向键
          const round = _this.rightRound(sharpper[0], sharpper[1])
          let ifStop
          for (let index = 0; index < round.length; index++) {
            if (round[index] !== 'view') {
              ifStop = true
            }
          }
          if (!ifStop) {
            _this.right()
          }
        }
      }
    },
    // 向下移动
    down () {
      const sharp = this.sharpper[0]
      this.sharpper[0].style.top = sharp.offsetTop + 30 + 'px'
    },
    // 向左移动
    left () {
      const left = this.sharpper[0].offsetLeft
      this.sharpper[0].style.left = left - 30 + 'px'
    },
    // 向右移动
    right () {
      const left = this.sharpper[0].offsetLeft
      this.sharpper[0].style.left = left + 30 + 'px'
    }
  }
}

</script>
<style >
*{
  margin: 0;
  padding: 0;
}
.full{
  width: 375px;
}
.point{
  position: absolute;
  display: block;
  background: yellow;
  height: 30px;
  width: 30px;
  z-index: 2;
}
.table{
  margin: auto;
  background: red;
  width: 300px;
  height: 600px;
  z-index: 99;
  overflow: hidden;
}
.view{
  margin: auto;
  background: #000;
  width: 300px;
  height: 600px;
  position: relative;
}
.sharp{
  position: absolute;
  top: 0;
  left:120px;
  transform-origin: 0 0;
}
/* ****************************************************************************************** */
.O{
  width: 60px;
  height: 60px;
  background: blue;
}
/* ****************************************************************************************** */
.I{
  width: 30px;
  height: 120px;
  background: Cyan;
}
/* ****************************************************************************************** */
.Z{
  width: 60px;
  height: 30px;
  background: red;
}
.Z::before{
  display: block;
  content: "";
  width: 60px;
  height: 30px;
  position: absolute;
  top: 30px;
  left: 30px;
  background: red;
}
/* ****************************************************************************************** */
.S{
  width: 60px;
  height: 30px;
  background: green;
}
.S::before{
  display: block;
  content: "";
  width: 60px;
  height: 30px;
  position: absolute;
  top: 30px;
  right: 30px;
  background: green;
}
/* ****************************************************************************************** */
.J{
  display: block;
  width: 30px;
  height: 90px;
  background: yellow;
}
.J:before{
  display: block;
  content: "";
  position: absolute;
  top: 60px;
  right: 30px;
  width: 30px;
  height: 30px;
  background: yellow;
}
/* ****************************************************************************************** */
.L{
  display: block;
  width: 30px;
  height: 90px;
  background: orange;
  }
.L:before{
  display: block;
  content: "";
  position: absolute;
  top: 60px;
  left: 30px;
  width: 30px;
  height: 30px;
  background: orange;
}
/* ****************************8 */
.T{
  display: block;
  width: 90px;
  height: 30px;
  background: purple;
}
.T::before{
  display: block;
  content: "";
  width: 30px;
  height: 30px;
  position: absolute;
  top: -30px;
  left: 30px;
  background: purple;
}
</style>
