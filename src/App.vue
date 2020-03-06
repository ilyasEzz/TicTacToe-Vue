<template>
  <div id="app">
    <div class="centered" v-if="rowsNum">
      <div v-bind:key="i" v-for="(n, i) in rowsNum">
        <div v-bind:key="j" v-for="(n, j) in rowsNum">
          <Cell v-on:performMove="performMove(i, j)" :value="board[i][j]" v-bind:i="i" v-bind:j="j"></Cell>
        </div>
      </div>
    </div>

    <div v-else>
      <form @submit.prevent="start">
        <div>
          <label for="player1">Player 1</label>
          <input v-model="player1" type="text" name="player1" />
        </div>
        <div>
          <label for="player2">Player 2</label>
          <input v-model="player2" type="text" name="player2" />
        </div>
        <div>
          <label for="rowsNum">Number of rows</label>
          <br />
          <input type="number" min="2" id="rowsNum" required />
        </div>
        <div>
          <label for="AI">Play with Computer</label>
          <input type="checkbox" v-model="AI" id="AI" />
        </div>
        <input type="submit" value="Play" class="btn" />
      </form>
    </div>
    <h1 v-if="message !== ''">{{ message }}</h1>
  </div>
</template>

<script>
import Cell from "./components/Cell.vue";

export default {
  name: "App",
  components: {
    Cell
  },
  data() {
    return {
      rowsNum: null,
      player1: "",
      player2: "",
      board: [],
      turnCounter: 0,
      diagonal: [],
      reverseDiagonal: [],
      win: false,
      message: "",
      AI: false
    };
  },
  methods: {
    start() {
      let num = document.querySelector("#rowsNum").value;
      num = Number.parseInt(num);

      // create board
      let matrix = [];
      for (let i = 0; i < num; i++) {
        matrix[i] = [];
        for (let j = 0; j < num; j++) {
          matrix[i].push("");
        }
      }
      this.board = matrix;

      this.rowsNum = num;
    },
    performMove(x, y) {
      if (!this.win) {
        if (this.board[x][y] !== "") return null;

        this.turnCounter++;

        if (this.turnCounter % 2 !== 0) {
          this.board[x][y] = "x";
        } else {
          this.board[x][y] = "o";
        }

        this.aiPlay();
        this.checkRowsCols();
        this.checkDiagonals(x, y);

        this.$forceUpdate();
      }
      this.displayWinner();
    },

    checkDiagonals(x, y) {
      // Diagonal
      if (x === y) {
        this.diagonal.push(this.board[x][y]);
        if (
          this.areEqual(this.diagonal) &&
          this.diagonal.length === this.board.length
        ) {
          this.win = true;
        }
      }

      //Reverse Diagonal
      if (this.board.length - 1 - x === y) {
        this.reverseDiagonal.push(this.board[x][y]);
        if (
          this.areEqual(this.reverseDiagonal) &&
          this.reverseDiagonal.length === this.board.length
        ) {
          this.win = true;
        }
      }
    },

    checkRowsCols() {
      const transposedBoard = this.transpose(this.board);

      for (let i = 0; i < this.board.length; i++) {
        // Vertical Check
        if (this.areEqual(this.board[i])) {
          this.win = true;
        }

        // Horizontal Check
        if (this.areEqual(transposedBoard[i])) {
          this.win = true;
        }
      }
    },
    transpose(matrix) {
      return matrix[0].map((_, c) => matrix.map(r => r[c]));
    },

    areEqual(arr) {
      if (arr.every(value => value === arr[0] && value !== "")) {
        return true;
      } else {
        return false;
      }
    },
    aiPlay() {
      this.turnCounter++;
      for (let i = 0; i < this.board.length; i++) {
        let empty = this.board[i].findIndex(value => value === "");
        if (empty === -1) {
          empty++;
          i++;
        }
        this.board[i][empty] = "o";

        console.log(i, empty);
        return;
      }
    },

    displayWinner() {
      if (!this.win) return;

      if (this.turnCounter % 2 !== 0) {
        this.message = `Player 1: ${this.player1} Won!`;
      } else {
        this.message = `Player 2: ${this.player2} Won!`;
      }
    },
    emptySpot() {
      for (let i = 0; i < this.board.length; i++) {
        let emptyRow = this.board[i].filter(value => value === "");
        console.log(emptyRow);
      }
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
