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
document.onkeydown = function (event) {
  var e = event || window.event
  if (e && e.keyCode === 37) { // 按 ←
    console.log(e.keyCode)
  }
  if (e && e.keyCode === 39) { // 按 →
    console.log(e.keyCode)
  }
  if (e && e.keyCode === 38) {
    console.log(e.keyCode)
  }
}
export default {
  name: 'Table',
  data () {
    return {}
  },
  mounted () {
    this.start()
  },
  methods: {
    start () {
      var view = this.$refs.view
      var type = Math.floor(Math.random() * 6)
      var sharp = this.create(type)
      view.appendChild(sharp)
      this.fall(sharp, type)
    },
    // 创建方块
    create (type) {
      var sharp = ''
      sharp = document.createElement('div')
      if (type === 0) {
        sharp.classList.add('O')
      } else if (type === 1) {
        sharp.classList.add('I')
      } else if (type === 2) {
        sharp.classList.add('Z')
      } else if (type === 3) {
        sharp.classList.add('S')
      } else if (type === 4) {
        sharp.classList.add('J')
      } else if (type === 5) {
        sharp.classList.add('L')
      } else if (type === 6) {
        sharp.classList.add('T')
      }
      return sharp
    },
    // 下坠
    fall (sharp, type) {
      var round = []
      switch (type) {
        case 0:
          // 初始化O形标白
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.500, sharp.offsetTop + sharp.clientHeight + 1).className)
          break
        case 1:
          // 初始化I形标白
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.500, sharp.offsetTop + sharp.clientHeight + 1).className)
          break
        case 2:
          // 初始化Z形标白
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15, sharp.offsetTop + sharp.clientHeight + 15).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15 + 30, sharp.offsetTop + sharp.clientHeight + 15 + 30).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15 + 60, sharp.offsetTop + sharp.clientHeight + 15 + 30).className)
          break
        case 3:
          // 初始化S形标白
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 - 15, sharp.offsetTop + sharp.clientHeight + 15 + 30).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15 + 30, sharp.offsetTop + sharp.clientHeight + 15 + 30).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 45, sharp.offsetTop + sharp.clientHeight + 15).className)
          break
        case 4:
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 - 15, sharp.offsetTop + sharp.clientHeight + 15).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15, sharp.offsetTop + sharp.clientHeight + 15).className)
          break
        case 5:
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15, sharp.offsetTop + sharp.clientHeight + 15).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 45, sharp.offsetTop + sharp.clientHeight + 15).className)
          break
        case 6:
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15, sharp.offsetTop + sharp.clientHeight + 15).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 45, sharp.offsetTop + sharp.clientHeight + 15).className)
          round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 75, sharp.offsetTop + sharp.clientHeight + 15).className)
          break
      }
      var i = setInterval(function () {
        let ifmove = ''
        // 判断标白处是否是view
        for (let index = 0; index < round.length; index++) {
          if (round[index] !== 'view') {
            ifmove = 'stop'
          }
        }
        // 判断是否停止
        if (ifmove !== 'stop') {
          sharp.style.top = sharp.offsetTop + 30 + 'px'
          round = []
          switch (type) {
            case 0:
              // O形需要一个下方标白
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.500, sharp.offsetTop + sharp.clientHeight + 1).className)
              break
            case 1:
              // I形需要一个下方标白
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.500, sharp.offsetTop + sharp.clientHeight + 1).className)
              break
            case 2:
              // Z形需要三个下方标白
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15, sharp.offsetTop + sharp.clientHeight + 15).className)
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15 + 30, sharp.offsetTop + sharp.clientHeight + 15 + 30).className)
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15 + 60, sharp.offsetTop + sharp.clientHeight + 15 + 30).className)
              break
            case 3:
              // S形需要三个下方标白
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 - 15, sharp.offsetTop + sharp.clientHeight + 15 + 30).className)
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15 + 30, sharp.offsetTop + sharp.clientHeight + 15 + 30).className)
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 45, sharp.offsetTop + sharp.clientHeight + 15).className)
              break
            case 4:
              // J形需要二个下方标白
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 - 15, sharp.offsetTop + sharp.clientHeight + 15).className)
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15, sharp.offsetTop + sharp.clientHeight + 15).className)
              break
            case 5:
              // L形需要二个下方标白
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15, sharp.offsetTop + sharp.clientHeight + 15).className)
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 45, sharp.offsetTop + sharp.clientHeight + 15).className)
              break
            case 6:
              // T形需要三个下方标白
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 15, sharp.offsetTop + sharp.clientHeight + 15).className)
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 45, sharp.offsetTop + sharp.clientHeight + 15).className)
              round.push(document.elementFromPoint(sharp.offsetLeft + 37.5 + 75, sharp.offsetTop + sharp.clientHeight + 15).className)
              break
          }
        } else {
          ifmove = 0
          clearInterval(i)
        }
      }, 1000)
    },
    left () {
      var actived = document.getElementsByClassName('actived')[0]
      console.log(actived)
    }
  }
}

</script>
<style >
*{
  margin: 0;
  padding: 0;
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
  position: relative;
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
  position: relative;
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
  position: relative;
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
  position: relative;
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
  position: relative;
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
