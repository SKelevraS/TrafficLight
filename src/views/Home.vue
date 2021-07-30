<template>
  <div class="container">
    <Lamp class="lamp" color="red" :currentColorIs="currentColorIs" :sec="counter"/>
    <Lamp class="lamp" color="yellow" :currentColorIs="currentColorIs" :sec="counter"/>
    <Lamp class="lamp" color="green" :currentColorIs="currentColorIs" :sec="counter"/>
  </div>
</template>
<script>
import Lamp from '@/components/Lamp.vue';
export default {
  name: 'home',
  props: ['currentColorIs', 'time'],
  components: {
    Lamp,
  },
  data() {
    return {
      counter: null,
      interval: null,
    };
  },
  computed: {
    getNextColor() {
      this.$store.commit('setNextColor', this.currentColorIs);
      return this.$store.state.nextColor;
    },
  },

  // Не позволяет таймеру быть меньше 0
  watch: {
    // eslint-disable-next-line no-unused-vars
    $route(to, from) {
      this.setTimer();
    },
  },  

  
  mounted() {
    this.setTimer();
  },
  methods: {
    //Создание таймера при переключении цвета
    setTimer() {
      const timer = this.time;

      setTimeout(() => { this.$router.push(this.getNextColor); }, timer);

      this.counter = this.time / 1000;

      if (this.interval) clearInterval(this.interval);

      this.interval = setInterval(() => {
        this.counter -= 1;
      }, 1000);

    }
  }
};
</script>

<style>
  .container {
    border: 1px solid grey;
    border-radius: 5px;
    width: 80px;
    margin: 275px auto;
    padding: 10px;
    background-color: grey;
  }
  .lamp {
    width: 60px;
    height: 60px;
    margin: 5px auto;
    border-radius: 50%;
    display: flex;
    justify-content: center;
  }
</style>
