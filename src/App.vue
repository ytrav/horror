<template>
  <!-- <img src="./assets/pano.png"> -->
  <div id="pano" :style="'transform: scale(1.5) translateX('+pos+'px)'">
    <span v-if="start">Apartment</span>
    <span v-if="start" @click="funcShowTimer">{{ msg }}</span>
    <span v-if="show">{{ msg2 }}</span>
    <span v-if="showTimer" id="timer">0:{{ visualTimer }}</span>
    <article @click="hide('bed')" id="bed">bed</article>
    <article @click="hide('table')" id="table">table</article>
    <article @click="hide('door')" id="door">door</article>
  </div>
  <div @mouseover="startLoopLeft(1, 2)" @mouseleave="clearLeft" class="leftZone"></div>
  <div @mouseover="startLoopRight(1, 2)" @mouseleave="clearRight" class="rightZone"></div>
  <div @mouseover="startLoopLeft(5, 1)" @mouseleave="clearLeft" class="LightleftZone"></div>
  <div @mouseover="startLoopRight(5, 1)" @mouseleave="clearRight" class="LightrightZone"></div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      pos: 0,
      msg: '>start',
      msg2: 'hide.',
      timer: 59,
      visualTimer: '59',
      show: false,
      showTimer: false,
      start: true,
      started: false,
    }
  },
  components: {
  },
  methods: {
    funcShowTimer() {
      if (this.started === false) {
        this.started = true;
        this.show = true;
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
          // alert('you die')
          that.showTimer = false;
          that.start = false;
          that.msg2 = 'too late.'
          that.visualTimer = '00';
          clearInterval(that.timerInterval)
          that.visualTimer = '00';
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
      console.log('called');
      var that = this;
      this.leftInterval = setInterval(function () {
        that.pos += speed;
        if (that.pos >= 400) {
          that.pos -= 1920;
        }
        console.log('repeated')
      }, inter);
    },
    startLoopRight(inter, speed) {
      console.log('called');
      var that = this;
      this.rightInterval = setInterval(function () {
        that.pos -= speed;
        if (that.pos <= -1832) {
          that.pos += 1920;
        }
        console.log('repeated')
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
  background-color: rgba(240, 248, 255, 0.048);
}

.LightleftZone,
.LightrightZone {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 200px;
  background-color: rgba(240, 248, 255, 0.055);
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
  }

  #timer {
    font-size: 1em;
    color: #dc143c;
  }
  #bed {
    background-color: rgba(255, 255, 255, 0.596);
    width: 200px;
    height: 320px;
    position: absolute;
    top: 56vh;
    left: 31%;
    transform: scale3d(1, 1, 1) rotateX(30deg) rotateY(0deg) rotateZ(38deg) translate3d(-11px, 0px, 0px) skew(-5deg, 11deg);
  }

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
}

body {

  overflow: hidden;
}

@media only screen and (min-width: 2221px) {
  #pano span {
    left: 49.1%;
  }
}

@media only screen and (max-width: 1645px) {
  #pano span {
    left: 48.8%;
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
