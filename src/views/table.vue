<template>
  <div class="full">
    <div ref="point" class="point">用来定位当前坐标的方块</div>
    <div ref="table" id="table" class="table">
      <div
        ref="view"
        @onkeydown.left="left"
        @onkeydown.right="right"
        class="view">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Table',
  data () {
    return {
      activiedSharp: ''
    }
  },
  mounted () {
    this.start()
  },
  methods: {
    start () {
      const sharpper = this.create()
      const sharp = sharpper[0]
      const type = sharpper[1]
      const round = this.downRound(sharp, type)
      this.keydown(sharpper)
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
          // 判断当前方块是否到底，如果到底停止并移除activied，反之坠落
          if (ifmove !== 'stop') {
            this.down(sharpper)
            round = []
          } else {
            ifmove = 0
            clearInterval(fall)
            this.start()
          }
        }, 500)
      }
    },
    // 创建方块
    create () {
      var view = this.$refs.view
      var sharp = ''
      var type = Math.floor(Math.random() * 6)
      sharp = document.createElement('div')
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
      view.appendChild(sharp)
      sharp.style.top = '0px'
      const sharpper = [sharp, type]
      return sharpper
    },
    // 设置下周围
    downRound (sharp, type) {
      const round = []
      switch (type) {
        case 0:
          // 设置O形周围
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5, sharp.offsetTop + sharp.clientHeight + 1).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 67.5, sharp.offsetTop + sharp.clientHeight + 1).className)
          break
        case 1:
          // 设置I形周围
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5, sharp.offsetTop + sharp.clientHeight + 1).className)
          break
        case 2:
          // 设置Z形周围
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15, sharp.offsetTop + sharp.clientHeight + 15).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15 + 30, sharp.offsetTop + sharp.clientHeight + 15 + 30).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15 + 60, sharp.offsetTop + sharp.clientHeight + 15 + 30).className)
          break
        case 3:
          // 设置S形周围
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 - 15, sharp.offsetTop + sharp.clientHeight + 15 + 30).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15, sharp.offsetTop + sharp.clientHeight + 15 + 30).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 45, sharp.offsetTop + sharp.clientHeight + 15).className)
          break
        case 4:
          // 设置J形周围
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 - 15, sharp.offsetTop + sharp.clientHeight + 15).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15, sharp.offsetTop + sharp.clientHeight + 15).className)
          break
        case 5:
          // 设置L形周围
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15, sharp.offsetTop + sharp.clientHeight + 15).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 45, sharp.offsetTop + sharp.clientHeight + 15).className)
          break
        case 6:
          // 设置T形周围
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15, sharp.offsetTop + sharp.clientHeight + 15).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 45, sharp.offsetTop + sharp.clientHeight + 15).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 75, sharp.offsetTop + sharp.clientHeight + 15).className)
          break
      }
      return round
    },
    // 设置左周围
    leftRound (sharp, type) {
      const left = sharp.offsetLeft + 37.5
      const top = sharp.offsetTop
      const round = []
      switch (type) {
        case 0:
          // 设置O形周围
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 - 15, sharp.offsetTop + 15).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 - 15, sharp.offsetTop + 45).className)
          break
        case 1:
          // 设置I形周围
          round.push(document.elementFromPoint(left - 15, sharp.offsetTop + 15).className)
          round.push(document.elementFromPoint(left - 15, sharp.offsetTop + 45).className)
          round.push(document.elementFromPoint(left - 15, sharp.offsetTop + 75).className)
          round.push(document.elementFromPoint(left - 15, sharp.offsetTop + 105).className)
          break
        case 2:
          // 设置Z形周围
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 - 15, sharp.offsetTop + 15).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15, sharp.offsetTop + 45).className)
          break
        case 3:
          // 设置S形周围
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 - 15, sharp.offsetTop + 15).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 - 45, sharp.offsetTop + 45).className)
          break
        case 4:
          // 设置J形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left - 15, top + 45).className)
          round.push(document.elementFromPoint(left - 45, top + 75).className)
          break
        case 5:
          // 设置L形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left - 15, top + 45).className)
          round.push(document.elementFromPoint(left - 15, top + 75).className)
          break
        case 6:
          // 设置T形周围
          round.push(document.elementFromPoint(left - 15, top + 15).className)
          round.push(document.elementFromPoint(left + 15, top - 15).className)
          break
      }
      return round
    },
    // 设置右周围
    // 监听键盘
    keydown (sharpper) {
      const _this = this
      document.onkeydown = function (e) {
        const key = window.event.keyCode
        if (key === 38) {
          console.log(sharpper[0])
        } else if (key === 37) {
          const round = _this.leftRound(sharpper[0], sharpper[1])
          let ifStop
          for (let index = 0; index < round.length; index++) {
            if (round[index] !== 'view') {
              ifStop = true
            }
          }
          console.log(round)
          if (!ifStop) {
            _this.left(sharpper[0])
          }
        } else if (key === 39) {
          const left = sharpper[0].offsetLeft
          sharpper[0].style.left = left + 30 + 'px'
        }
      }
    },
    // 向下移动
    down (sharpper) {
      const sharp = sharpper[0]
      sharp.style.top = sharp.offsetTop + 30 + 'px'
    },
    // 向左移动
    left (sharp) {
      const left = sharp.offsetLeft
      sharp.style.left = left - 30 + 'px'
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
  background: #000;
  width: 300px;
  height: 600px
}
.view{
  margin: auto;
  background: #000;
  width: 300px;
  height: 600px;
  position: relative;
}
/* ****************************************************************************************** */
.O{
  width: 60px;
  height: 60px;
  background: blue;
  position: absolute;
  top: 0;
  left:120px
}
/* ****************************************************************************************** */
.I{
  width: 30px;
  height: 120px;
  background: Cyan;
  position: absolute;
  top: 0;
  left:120px
}
/* ****************************************************************************************** */
.Z{
  width: 60px;
  height: 30px;
  background: red;
  position: absolute;
  top: 0;
  left:120px
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
  position: absolute;
  top: 0;
  left:120px
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
  position: absolute;
  top: 0;
  left:120px
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
  position: absolute;
  top: 0;
  left:120px
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
  position: absolute;
  top: 0;
  left:120px
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
