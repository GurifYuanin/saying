<template>
<div class="one-saying"
     :style="{
      transition: 'all ' + time + 's linear',
      bottom: bottom + 'px',
      left: left + 'px'
    }"
>
  <div class="username">username</div>
  <div class="content">content</div>
</div>
</template>

<script>
export default {
  props: {

  },
  data () {
    return {
      bottom: 0,
      left: 0,
      speed: 800,
      deg: 0.33, // 2pi / 4 * 3, 270deg
      distence: 1600,
      vw: 1400,
      vh: 293,
      d90: Math.PI / 2,
      d180: Math.PI,
      d270: Math.PI / 2 * 3,
      d360: Math.PI * 2
    };
  },
  computed: {
    time () {
      return this.distence / this.speed;
    }
  },
  methods: {
    update () {

      let xSpeed, ySpeed;
      if (this.deg < this.d90) {
        xSpeed = Math.cos(this.deg) * this.speed;
        ySpeed = Math.sin(this.deg) * this.speed;
      } else if (this.deg < this.d180) {
        xSpeed = - Math.sin(this.deg - this.d90) * this.speed;
        ySpeed = Math.cos(this.deg - this.d90) * this.speed;
      } else if (this.deg < this.d270) {
        xSpeed = - Math.cos(this.deg - this.d180) * this.speed;
        ySpeed = - Math.sin(this.deg - this.d180) * this.speed;
      } else if (this.deg < this.d360) {
        xSpeed = Math.sin(this.deg - this.d270) * this.speed;
        ySpeed = - Math.cos(this.deg - this.d270) * this.speed;
      }
      let isHorizen = true;
      let w = (this.deg < this.d90 || this.deg > this.d270 || this.left === 0) ?
              (this.vw - (this.vw === this.left ? 0 : this.left)) / Math.abs(xSpeed) :
              this.left / Math.abs(xSpeed);
      let h = (this.deg > this.d90 && this.deg < this.d180 || this.bottom == 0) ?
              (this.vh - (this.vh === this.bottom ? 0 : this.bottom)) / Math.abs(ySpeed) :
              this.bottom / Math.abs(ySpeed);
      let cost = Math.min(w, h);
      isHorizen = h < w;
      this.bottom = Math.abs(this.bottom + ySpeed * cost);
      this.left = Math.abs(this.left + xSpeed * cost);
      if (this.deg < 0) {
      } else if (this.deg < this.d90) {
        this.deg = isHorizen ? this.d270 + this.deg : this.d90 + this.deg;
      } else if (this.deg < this.d180) {
        this.deg = isHorizen ? this.deg + this.d90 : this.deg - this.d90;
      } else if (this.deg < this.d270) {
        this.deg = isHorizen ? this.deg - this.d90 : this.deg + this.d90;
      } else if (this.deg < this.d360) {
        this.deg = isHorizen ? this.deg - this.d270 : this.deg - this.d90;
      }
    }
  },
  created () {
    setInterval(this.update, this.time * 1000);
    this.vw = window.innerWidth - 200;
    this.vh = window.innerHeight - 200;
  }
}
</script>

<style>
.one-saying {
  position: absolute;
  width: 200px;
  height: 200px;
  border: solid 1px black;

  background-color: white;

}
.username {
  border-bottom: solid 1px black;
}
.content {

}
</style>
