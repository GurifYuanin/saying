<template>
<div style="height: 100%">
  <div id="mask" @click="$emit('toggleModal')"></div>
  <div id="form-container">
    <div class="form">
      <div class="title">username</div>
      <div><input class="field" type="text" v-model="username"></div>
      <div class="title">content</div>
      <div><textarea class="field" cols="30" rows="10" v-model="content"></textarea></div>
      <div><button class="field button" @click="add">添加</button></div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  data () {
    return {
      username: '',
      content: ''
    };
  },
  methods: {
    add () {
      if (this.username && this.content) {
        this.$emit('add', {
          username: this.username,
          content: this.content
        });
        this.username = '';
        this.content = '';
      } else {
        alert('请填写信息！');
      }
    }
  }
}
</script>

<style scoped>
#mask {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  opacity: .5;

  background-color: black;
  z-index: 1;
}
#form-container {
  position: relative;
  top: 50%;
  height: 100%;

  z-index: 2;
}
.form {
  padding: 20px 40px;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);

  border-radius: 10px;
  box-shadow: 0 0 20px 0.5px rgba(255, 255, 255, .5);

  text-align: center;
  background-color: white;
}
.title {
  font-size: 24px;
  color: #222;
}
.field {
  width: 100%;

  border: dotted 1px #444;
  border-radius: 5px;

  line-height: 24px;
  font-size: 18px;
}
.button {
  border: none;

  background-color: #57cee6;
  color: white;

  cursor: pointer;
}
.button:hover {
  background-color: #2ac0de;
}
.button:active {
  background-color: #6fccde;
}
</style>
