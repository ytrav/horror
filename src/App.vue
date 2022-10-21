<template>
  <div v-if="menu" class="startMenu">
    <h1>Apartment</h1>
    <h2>an experience by ytrav, assets by Viktor Kysil</h2>
    <h3 @click="startGame">Begin</h3>
  </div>
  <Transition>
    <div v-if="end" id="end">
      <h1>{{ end1 }}</h1>
      <h2>{{ end2 }}</h2>
    </div>
  </Transition>

  <div v-show="showTimer" id="tooltip">{{ tooltip }}</div>
  <!-- <img src="./assets/pano.png"> -->
  <div id="pano" :class="{invisible: hidePano}" :style="'transform: scale(1.5) translateX('+pos+'px)'">
    <span v-if="start">Apartment</span>
    <span :class="{active: !show}" v-if="start" @click="funcShowTimer">{{ msg }}</span>
    <span v-if="show">{{ msg2 }}</span>
    <span v-if="showTimer" id="timer">0:{{ visualTimer }}</span>
    <article v-if="showTimer" @click="hide('bed')" id="bed"></article>
    <article v-if="showTimer" @click="hide('table')" id="table"></article>
    <article v-if="showTimer" @click="hide('door')" id="door"></article>
    <article v-if="showTimer" @click="escape" id="window"></article>
  </div>
  <div v-show="showTriggers" @mouseover="startLoopLeft(1, 2)" @mouseleave="clearLeft" class="leftZone"></div>
  <div v-show="showTriggers" @mouseover="startLoopRight(1, 2)" @mouseleave="clearRight" class="rightZone"></div>
  <div v-show="showTriggers" @mouseover="startLoopLeft(5, 1)" @mouseleave="clearLeft" class="LightleftZone"></div>
  <div v-show="showTriggers" @mouseover="startLoopRight(5, 1)" @mouseleave="clearRight" class="LightrightZone"></div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      menu: true,
      pos: 0,
      end1: 'you have escaped',
      end2: 'forever',
      hidePano: false,
      showTriggers: false,
      msg: 'start',
      msg2: 'hide.',
      timer: 59,
      visualTimer: '59',
      show: false,
      showTimer: false,
      start: true,
      started: false,
      tooltip: 'he will find you.',
      cursorX: -300,
      cursorY: -300,
      end: false,
    }
  },
  components: {
  },
  mounted() {
    document.getElementById("tooltip").style.left = this.cursorX + "px";
    document.getElementById("tooltip").style.top = this.cursorY + "px";
  },
  methods: {
    startGame() {
      var elem = document.documentElement;

      /* View in fullscreen */
      if (elem.requestFullscreen) {
        elem.requestFullscreen();
      } else if (elem.webkitRequestFullscreen) { /* Safari */
        elem.webkitRequestFullscreen();
      } else if (elem.msRequestFullscreen) { /* IE11 */
        elem.msRequestFullscreen();
      }
      this.menu = false;
      this.showTriggers = true;
      return;
    },
    escape() {
      clearInterval(this.timerInterval)
      this.showTriggers = false;
      this.pos = -200;
      document.getElementById("pano").style.zoom = "1";
      var that = this;
      setTimeout(function () {
        that.start = false;
        that.msg = '';
        that.msg2 = '';
        that.showTimer = false;
        document.getElementById("pano").style.zoom = "1.25";
      }, 500)
      setTimeout(function () {
        document.getElementById("pano").style.zoom = "1.5";
      }, 1000)
      setTimeout(function () {
        document.getElementById("pano").style.zoom = "1.75";
      }, 1500)
      setTimeout(function () {
        document.getElementById("pano").style.zoom = "3";
      }, 3000)
      setTimeout(function () {
        document.getElementById("pano").style.zoom = "4";
      }, 4000)
      setTimeout(function () {
        document.getElementById("pano").style.opacity = "0";
      }, 5000)
      setTimeout(function () {
        that.end = true;
        if (document.exitFullscreen) {
          document.exitFullscreen();
        } else if (document.webkitExitFullscreen) { /* Safari */
          document.webkitExitFullscreen();
        } else if (document.msExitFullscreen) { /* IE11 */
          document.msExitFullscreen();
        }
      }, 8000)
    },
    hide(way) {
      switch (way) {
        case 'bed':
          this.tooltip = 'he will find you under the bed'
          break;
        case 'door':
          this.tooltip = "DON'T"
          break;

        case 'table':
          this.tooltip = 'he will find you under the table'
          break;

        default:
          break;
      }
      // alert(way);
      this.cursorX = event.clientX;
      this.cursorY = event.clientY;
      document.getElementById("tooltip").style.left = this.cursorX + "px";
      document.getElementById("tooltip").style.top = this.cursorY + "px";
      setTimeout(function () {
        document.getElementById("tooltip").style.left = "-300px";
        document.getElementById("tooltip").style.top = "-300px";
      }, 2000)
      // alert(`${way}. X${this.cursorX}, Y${this.cursorY}`);
      // alert(document.getElementById("tooltip").style.left + document.getElementById("tooltip").style.top);
    },
    funcShowTimer() {
      if (this.started === false) {
        this.started = true;
        this.show = true;
        this.msg = '>start'
        let that = this;
        setTimeout(function () {
          that.showTimer = true;
          that.startTimer();
        }, 2000)
      }

    },
    startTimer() {
      this.timer = 59;
      var that = this;
      this.timerInterval = setInterval(function () {
        if (that.timer === 0) {
          // you die
          that.showTimer = false;
          that.start = false;
          that.msg2 = 'too late.'
          that.visualTimer = '00';
          clearInterval(that.timerInterval)
          that.visualTimer = '00';
          setTimeout(function () {
            that.hidePano = true;
          }, 5000)
          setTimeout(function () {
            that.msg2 = ''
          }, 12000)
          setTimeout(function () {
            that.end1 = 'you were too late';
            that.end2 = 'he found you';
            that.end = true;
            if (document.exitFullscreen) {
              document.exitFullscreen();
            } else if (document.webkitExitFullscreen) { /* Safari */
              document.webkitExitFullscreen();
            } else if (document.msExitFullscreen) { /* IE11 */
              document.msExitFullscreen();
            }
          }, 15000)

        }
        that.timer--
        if (that.visualTimer <= 10) {
          that.visualTimer = `0${that.timer}`
        } else {
          that.visualTimer = that.timer
        }
      }, 1000)
    },
    startLoopLeft(inter, speed) {
      var that = this;
      this.leftInterval = setInterval(function () {
        that.pos += speed;
        if (that.pos >= 400) {
          that.pos -= 1920;
        }
      }, inter);
    },
    startLoopRight(inter, speed) {
      var that = this;
      this.rightInterval = setInterval(function () {
        that.pos -= speed;
        if (that.pos <= -1328) {
          that.pos += 1920;
        }
      }, inter);
    },
    clearLeft() {
      clearInterval(this.leftInterval);
    },
    clearRight() {
      clearInterval(this.rightInterval);
    },
  }
}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
}

.leftZone,
.rightZone {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 150px;
  // background-color: rgba(240, 248, 255, 0.048);
}

.LightleftZone,
.LightrightZone {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 200px;
  // background-color: rgba(240, 248, 255, 0.055);
}

.LightleftZone {
  left: 150px;
}

.LightrightZone {
  right: 150px;
}

.leftZone {
  left: 0;
}

.rightZone {
  right: 0;
}

#end,
.startMenu {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  font-family: monospace;
  gap: 20px;
  color: crimson;
  font-weight: lighter;

  h1 {
    opacity: 0.9
  }

  h2 {
    opacity: 0.7
  }
}

.startMenu {
  color: #008cff !important;
  z-index: 150;
  background-color: #000;

  h3 {
    cursor: pointer;

    &:before {
      content: '\00a0';
    }

    &:hover:before {
      content: '>'
    }
  }
}

#pano {
  background-image: url(./assets/pano.png);
  position: absolute;
  top: 0;
  bottom: 0;
  right: -2000px;
  left: 0;
  background-position: center;
  // background-size: 100% 700px;
  // background-size: contain;
  background-repeat: repeat-x;
  overflow: visible;

  span {
    cursor: default;
    text-align: left;
    font-size: 0.6em;
    padding: 0;
    // margin: -4px;
    color: #fff;
    // background-color: #000;
    font-family: monospace;
    position: relative;
    // bottom: 1000px;
    top: 46.6vh;
    left: 49%;
    opacity: 0.7;
    color: rgb(205, 218, 255);
    // border: 1px solid gray;
    width: 93px;
    max-width: 93px;
    display: block;

    // &:nth-child(2) {
    //   top: 48vh;
    // }
    &.active {
      &:before {
        content: '\00a0'
      }

      cursor: pointer;

      &:hover:before {
        content: '>';
      }
    }
  }

  #timer {
    font-size: 1em;
    color: #dc143c;
  }

  #bed,
  #door,
  #table,
  #window {
    // background-color: rgba(255, 255, 255, 0.596);
    position: absolute;

  }

  #bed {
    width: 210px;
    height: 300px;
    top: 58vh;
    left: 31%;
    z-index: 8;
    transform: scale3d(1, 1, 1) rotateX(30deg) rotateY(0deg) rotateZ(38deg) translate3d(-11px, 0px, 0px) skew(-5deg, 11deg);
  }

  #door {
    width: 100px;
    height: 180px;
    top: 47vh;
    left: 24.5%;
  }

  #table {
    width: 380px;
    height: 120px;
    top: 63vh;
    left: 46%
  }

  #window {
    width: 250px;
    height: 150px;
    top: 42vh;
    left: 34.6%;
    transform: rotate(-8deg);
    z-index: 10;
  }

}

.invisible {
  background-image: none !important;
  background-color: transparent;
}

#tooltip {
  position: absolute;
  z-index: 12;
  color: #dc143c;
  font-family: monospace;
  background-color: rgb(32, 32, 32);
  padding: 5px;
  font-weight: 600;
}

// .spans {
//   display: flex;
//   flex-direction: column;
// }

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}

html {
  overflow: hidden;
  background-color: #000;
}

body {

  overflow: hidden;
}

@media only screen and (min-width: 2221px) {
  #pano span {
    left: 49.1%;
  }

  #pano {
    #window {
      left: 36%;
    }

    #bed {
      left: 33%;
    }

    #door {
      left: 26%;
    }
  }
}

@media only screen and (max-width: 1645px) {
  #pano span {
    left: 48.8%;
  }

  #pano {
    #window {
      left: 33%;
    }

    #bed {
      left: 30%;
    }

    #door {
      left: 23%;
    }
  }
}

@media only screen and (max-width: 1293px) {
  #pano span {
    left: 48.7%;
  }
}

@media only screen and (min-height: 1156px) {
  #pano span {
    top: 47.2vh;
  }
}


@keyframes shake {

  10%,
  90% {
    transform: translate3d(-1px, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(2px, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-4px, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(4px, 0, 0);
  }
}
</style>
