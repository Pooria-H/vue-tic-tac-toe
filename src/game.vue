<template>
  <div id="app">
    <div class="bg-black text-white row">
      <h1>
        XO Game
      </h1>
    </div>
    <div class="float">
      <board @gameFinished="onGameFinish" :bus="bus" :first-player="firstPlayer" :is-game-finished="isGameFinished"></board>
    </div>
    <div class="float">
      <div class="row">
        <b>Round:</b> <span>{{ numberOfGames }}</span>
      </div>
      <div class="row">
        <b>Player X wins:</b> <span>{{ x }}</span>
      </div>
      <div class="row">
        <b>Player O Wins:</b> <span>{{ o }}</span>
      </div>
      <div class="row">
        <button @click="resetGame" class="btn" type="button">
          Reset Stats
        </button>
        <button @click="createNewGame" v-if="isGameFinished" class="btn" type="button">
          New game
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
import board from './components/board';

export default {
  name: 'game',
  components: {
    board,
  },
  methods: {
    onGameFinish(winner) {
      this.isGameFinished = true;
      if (winner === 'X') {
        this.x += 1;
      } else if (winner === 'O') {
        this.o += 1;
      }
    },
    createNewGame() {
      this.isGameFinished = false;
      this.numberOfGames += 1;
      if (this.numberOfGames % 2 === 0) {
        this.firstPlayer = 'O';
      } else {
        this.firstPlayer = 'X';
      }
      this.bus.$emit('createNewGame');
    },
    resetGame() {
      this.x = 0;
      this.o = 0;
      this.numberOfGames = 0;
      this.createNewGame();
    },
  },
  data () {
    return {
      bus: new Vue(),
      x: 0,
      o: 0,
      numberOfGames: 1,
      firstPlayer: 'X',
      isGameFinished: false,
    };
  },
}
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}
html, body {
  font-family: 'Courier New', Courier, monospace;
}
body {
  margin: 0;
  padding: 30px;
  background-color: #d0cece;
}
h1 {
  padding: 5px 10px;
}
.float {
  float: left;
  padding: 0 10px;
  min-width: 35%;
}
.row {
  margin-bottom: 10px;
}
.bg-black {
  background-color: black;
}
.text-white {
  color: white;
}
.upperCase {
  text-transform: uppercase;
}
.btn {
  font-size: 1.1em;
  padding: 5px 10px;
  cursor: pointer;
  background-color: white;
  border: 1px solid #c5c5c5;
}
</style>
