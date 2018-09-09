<template>
<div id="operation">
  <div class="add" @click="add" @title="添加一条">+</div>
</div>
</template>

<script>
export default {
  data () {
    return {

    };
  },
  methods: {
    add () {
      this.$emit('add');
    }
  }
}
</script>

<style scoped>
#operation {
  height: 50px;
  position: absolute;
  right: 0;
  top: 0;
  transform: translate(50%, 100%);
  transition: all .5s;
}
#operation:hover {
  transform: translate(0%, 100%);
}
.add {
  width: 50px;
  height: 50px;
  background-color: white;
  border-radius: 50%;
  border: dotted .5px gray;
  font-size: 50px;
  line-height: 40px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  transition: all .5s;
  color: black;
}
.add:hover {
  background-color: #57cee6;
  color: white;
}
</style>
