<template>
<div id="range-container">
  <button class="button" @click="number === 0 ? '' : number--">-1</button>
  <input class="range" v-model="number" type="range" min="0" max="100">
  <button class="button" @click="number === 100 ? '' : number++">+1</button>
  <div>显示数量：{{number < 10 ? '0' + number : number}}</div>
</div>
</template>

<script>
export default {
  data () {
    return {
      number: 1,
      timer: null
    }
  },
  watch: {
    number () {
      if (this.timer) clearTimeout(this.timer);
      this.timer = setTimeout(() => {
        this.$emit('filterComments', this.number);
      }, 500);
    }
  }
}
</script>

<style scoped>
#range-container {
  position: absolute;
  top: 0;

  width: 100%;
  text-align: center;

  z-index: 1;

  transition: all .5s;
  opacity: .5;
}
#range-container:hover {
  opacity: 1;
}
.range {
  -webkit-appearance: none;
  border-radius: 10px;
  cursor: pointer;
}
.range:focus {
  outline: none;
}
.range::-webkit-slider-thumb, .range::-moz-range-thumb {
  -webkit-appearance: none;
}
.range::-webkit-slider-runnable-track {
  height: 15px;
  border-radius: 10px; /* 将轨道设为圆角的 */
  box-shadow: 0 1px 1px #def3f8, inset 0 .125em .125em #0d1112; /* 轨道内置阴影效果 */
}
.range::-moz-range-track {
  height: 15px;
  border-radius: 10px; /* 将轨道设为圆角的 */
  box-shadow: 0 1px 1px #def3f8, inset 0 .125em .125em #0d1112; /* 轨道内置阴影效果 */
}
.range::-webkit-slider-thumb {
    -webkit-appearance: none;
    height: 25px;
    width: 25px;
    margin-top: -5px; /* 使滑块超出轨道部分的偏移量相等 */
    background: #ffffff;
    border-radius: 50%; /* 外观设置为圆形 */
    border: solid 0.125em rgba(205, 224, 230, 0.5); /* 设置边框 */
    box-shadow: 0 .125em .125em #3b4547; /* 添加底部阴影 */
}
.range::-moz-range-progress {
    background: linear-gradient(to right, #059CFA, white 100%, white);
    height: 13px;
    border-radius: 10px;
}

.range::-ms-track {
    height: 25px;
    border-radius: 10px;
    box-shadow: 0 1px 1px #def3f8, inset 0 .125em .125em #0d1112;
    border-color: transparent; /* 去除原有边框*/
    color: transparent; /* 去除轨道内的竖线 */
}

.range::-ms-thumb {
    border: solid 0.125em rgba(205, 224, 230, 0.5);
    height: 25px;
    width: 25px;
    border-radius: 50%;
    background: #ffffff;
    margin-top: -5px;
    box-shadow: 0 .125em .125em #3b4547;
}

.range::-ms-fill-lower {
    /* 进度条已填充的部分 */
    height: 22px;
    border-radius: 10px;
    background: linear-gradient(to right, #059CFA, white 100%, white);
}

.range::-ms-fill-upper {
    /* 进度条未填充的部分 */
    height: 22px;
    border-radius: 10px;
    background: #ffffff;
}
.range:focus::-ms-fill-lower {
    background: linear-gradient(to right, #059CFA, white 100%, white);
}
.range:focus::-ms-fill-upper {
    background: #ffffff;
}
.button {
  padding: 5px 10px;
  margin: 0 5px;

  border: none;
  border-radius: 5px;

  background-color:#3d80da;
  color:#fff;
  cursor: pointer;
}
.button:hover{
  background-color:#2673db
}
</style>
