<template>
  <div id="app">
    <Game v-if="rowsNum" v-bind:rowsNum="rowsNum" class="centered" />
    <div>
      <form @submit.prevent="start">
        <div>
          <label for="player1">Player 1 </label>
          <input v-model="player1" type="text" name="player1" />
        </div>
        <div>
          <label for="player2">Player 2 </label>
          <input v-model="player2" type="text" name="player2" />
        </div>
        <div>
          <label for="rowsNum">Number of rows </label> <br />
          <input
            v-model.number="rowsNum"
            type="number"
            min="2"
            id="rowsNum"
            required
          />
        </div>
        <input type="submit" value="Play" class="btn" />
      </form>
    </div>
  </div>
</template>

<script>
import Game from './components/Game.vue';

export default {
  name: 'App',
  components: {
    Game
  },
  data() {
    return {
      rowsNum: 3,
      player1: '',
      player2: ''
    };
  },
  methods: {
    start() {
      let num = document.querySelector('#rowsNum').value;
      num = Number.parseInt(num);
      this.rowsNum = num;
      console.log('start Game');
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.centered {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
