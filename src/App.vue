<template>
  <h1 id="title">Work Day Scheduler</h1>
  <p>A simple calender app for scheduling your work day</p>
  <p>{{ formated }}</p>
  <hr>
  <div class="container">
    <div class="time">
      <timeblock v-for="offset in 9" :key="offset" :unit="-4 + offset" :current="now"/>
    </div>
  </div>
</template>

<script>
import { computed, ref } from 'vue';
import { DateTime } from 'luxon';
import timeblock from './components/timeblock.vue';

export default
{
  name: 'App',
  components: { timeblock },
  setup() {
    const now = ref(DateTime.now());
    let prefix = ref('th');
    if (now.value.day <= 2) {
      prefix = now.value.day === 1 ? 'st' : 'nd';
    }

    // update the now value every minute
    setInterval(() => {
      now.value = DateTime.now();
    }, 60 * 1000);

    const formated = computed(() => now.value.toFormat('EEEE, MMMM d') + prefix.value);
    return { formated, now };
  },
};
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap');

  * {
    margin: 0;
    padding: 0;
    font-family: 'Roboto', sans-serif;
  }

  body {
    width: 100%;
    height: 100%;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    margin-top: 60px;
  }
</style>

<style lang="scss" scoped>
  #title {
    font-size: 4em;
  }

  p {
    font-size: 1.2em;
  }

  #title + p + p {
    margin-top: 10px;
    margin-bottom: 75px;
  }

  hr {
    margin-top: 50px;
    margin-bottom: 30px;
    height: 10px;
    background: black;
  }

  .container {
    width: 100%;
    height: 100%;
    display: grid;
    justify-content: center;

    .time {
      width: 75vw;
      display: flex;
      flex-direction: column;
    }
  }

</style>
