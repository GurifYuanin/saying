<template>
  <div id="app" :style="{backgroundPosition: bgX + 'px ' + bgY + 'px'}">
    <template v-for="el in comments">
      <Saying :opt="{
        speed: Math.random() * 800,
        deg: Math.random() * Math.PI * 2,
        left: Math.random() * vw,
        bottom: Math.random() * vh
      }"
            :username="el.username"
            :content="el.content"
            :vw="vw"
            :vh="vh"
      />
    </template>
    <Operation @add="add"/>
    <Modal v-show="modaling" @toggleModal="toggleModal" @add="addOne"/>
    <Range @filterComments="filterComments" />
  </div>
</template>

<script>
import axios from 'axios';
import Saying from '@/components/saying.vue';
import Operation from '@/components/operation.vue';
import Modal from '@/components/modal.vue';
import Range from '@/components/range.vue';
export default {
  name: 'App',
  data () {
    return {
      vw: window.innerWidth - 200,
      vh: window.innerHeight - 200,
      bgX: 0,
      bgY: 0,
      comments: process.env.NODE_ENV === 'development' ? [
        {username: '1', content: '1', saying_id: '1'}
      ] : [],
      modaling: false,
    };
  },
  methods: {
    add () {
      this.modaling = true;
    },
    addOne (item) {
      axios.post('/index.php/saying/Message/addSaying', item).then(response => {
        if (response.data.status) {
          this.comments.push(item);
          this.modaling = false;
        } else {
          alert('添加失败');
        }
      }, err => console.error(err));
    },
    toggleModal () {
      this.modaling = !this.modaling;
    },
    filterComments (num) {
      if (num < this.comments.length) {
        this.comments.splice(num);
      } else if (process.env.NODE_ENV === 'development') {
        while (this.comments.length < num) {
          this.comments.push({
            username: '何二狗',
            content: '你好呀',
            saying_id: this.comments.length
          });
        }
      } else {
        axios.get('/index.php/saying/Message/getSayingList?size=' + num).then(response => {
          this.comments = response.data;
        }).catch(err => console.error(err));
      }
    }
  },
  components: {
    Saying,
    Operation,
    Modal,
    Range
  },
  created () {
    // setInterval(() => {
    //   this.bgX = (this.bgX + 100) % Number.MAX_SAFE_INTEGER;
    //   this.bgY = (this.bgY + 100) % Number.MAX_SAFE_INTEGER;
    // }, 1000);
    window.addEventListener('resize', () => {
      this.vw = window.innerWidth - 200;
      this.vh = window.innerHeight - 200;
    });
    try {
      axios.get('/index.php/saying/Message/getSayingList').then(response => {
        this.comments = response.data;
      }).catch(err => console.error(err));
    } catch (e) {}
  },
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
  overflow: hidden;
}

#app {
  position:relative;
  margin: 0;
  padding: 0;
  height: 100%;
  height: 100vh;
  overflow: hidden;
  background-image: url('./assets/bg.png');
  background-repeat: repeat;
  transition: all 1s linear;
}
</style>
