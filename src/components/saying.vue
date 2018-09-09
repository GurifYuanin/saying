<template>
<div :class="['one-saying', isMoving ? '' : 'one-saying-clicked']"
     :style="{
      transition: 'all ' + time + 'ms linear',
      bottom: bottom + 'px',
      left: left + 'px'
    }"
    @click="toggleMove"
>
  <div class="username">{{username}}</div>
  <div class="content">{{content}}</div>
</div>
</template>

<script>
export default {
  props: {
    opt: {
      type: Object,
      default () {
        return {
          speed: {
            type: Number,
            default: 800
          },
          deg: {
            type: Number,
            default: 0.7
          },
          left: {
            type: Number,
            default: 0
          },
          bottom: {
            type: Number,
            default: 0
          }
        }
      }
    },
    vw: {
      type: Number,
      default: window.innerWidth - 200
    },
    vh: {
      type: Number,
      default: window.innerHeight - 200
    },
    username: {
      type: String,
      requred: true
    },
    content: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      bottom: 0,
      left: 0,
      speed: 800,
      deg: 0.7, // 2pi / 4 * 3, 270deg
      distence: 1600,
      timer: null,
      isMoving: true,
      d90: Math.PI / 2,
      d180: Math.PI,
      d270: Math.PI / 2 * 3,
      d360: Math.PI * 2
    };
  },
  computed: {
    time () {
      return this.distence / this.speed * 1000;
    }
  },
  methods: {
    update () {
      let xSpeed = Math.cos(this.deg) * this.speed,
          ySpeed = Math.sin(this.deg) * this.speed;
      let w = (this.deg < this.d90 || this.deg > this.d270) ?
              (this.vw - (this.vw === this.left ? 0 : this.left)) / Math.abs(xSpeed) :
              this.left / Math.abs(xSpeed);
      let h = (this.deg < this.d180) ?
              (this.vh - (this.vh === this.bottom ? 0 : this.bottom)) / Math.abs(ySpeed) :
              this.bottom / Math.abs(ySpeed);
      let cost = Math.min(w, h);
      let isHorizen = h < w;
      let nextBottom = this.bottom + ySpeed * cost,
          nextLet = this.left + xSpeed * cost;
      this.distence = Math.sqrt(Math.pow(nextBottom - this.bottom, 2) + Math.pow(nextLet - this.left, 2));
      this.bottom = nextBottom;
      this.left = nextLet;
      this.deg = isHorizen ?
                 this.d360 - this.deg :
                 (Math.PI * 3 - this.deg) % this.d360;
      this.timer = setTimeout(this.update, this.time);
    },
    toggleMove () {
      if (this.isMoving) {
        clearTimeout(this.timer);
        this.distence = 0;
      } else {
        this.timer = setTimeout(this.update, this.time);
      }
      this.isMoving = !this.isMoving;
    }
  },
  created () {
    this.timer = setTimeout(this.update, this.time);
    this.bottom = this.opt.bottom.default || this.opt.bottom;
    this.left = this.opt.left.default || this.opt.left;
    this.speed = this.opt.speed.default || this.opt.speed;
    this.deg = this.opt.deg.default || this.opt.deg;
  },
}
</script>

<style>
.one-saying {
  position: absolute;
  width: 200px;
  height: 200px;
  border: solid 1px #9e9e9e;

  background-color: white;
  border-radius: 10px;
  cursor: pointer;
  transition: all .25s;
}
.one-saying:active {
  transform: scale(.9);
}
.one-saying-clicked {
  box-shadow: 0 0 20px .5px #ccc;
}
.username {
  box-sizing: border-box;
  width: 200px;
  height: 32px;

  text-align: center;
  border-bottom: solid 1px #9e9e9e;
  border-radius: 10px 10px 0 0;

  font-size: 24px;
  font-weight: bold;

  color: #e7fffd;
  background-color: #57cee6;
}
.content {
  box-sizing: border-box;
  width: 200px;
  height: 168px;

  padding: 5px;
  border-radius: 0 0 10px 10px;
  overflow: hidden;

  font-size: 16px;
  line-height: 24px;
  word-wrap: break-word;
  text-overflow: ellipsis;

  color: #848484;
  background-color: #dffffd;
}
</style>
