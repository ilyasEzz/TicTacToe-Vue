<template>
  <div>
    <div v-bind:key="i" v-for="(n, i) in 3">
      <div v-bind:key="j" v-for="(n, j) in 3">
        <Cell
          v-on:performMove="performMove(i, j)"
          :value="board[i][j]"
          v-bind:i="i"
          v-bind:j="j"
        ></Cell>
      </div>
    </div>
  </div>
</template>

<script>
import Cell from './Cell';

export default {
  name: 'Game',
  components: {
    Cell
  },
  props: {
    rowsNum: Number
  },

  created() {
    // create board
    let matrix = [];
    for (let i = 0; i < this.rowsNum; i++) {
      matrix[i] = [];
      for (let j = 0; j < this.rowsNum; j++) {
        matrix[i].push('');
      }
    }
    this.board = matrix;
  },

  data() {
    return {
      rows: 3,
      board: [],
      turnCounter: 0,
      diagonal: [],
      reverseDiagonal: []
    };
  },

  methods: {
    performMove(x, y) {
      if (this.board[x][y] !== '') return null;

      this.turnCounter++;

      this.turnCounter % 2 !== 0
        ? (this.board[x][y] = 'x')
        : (this.board[x][y] = 'o');

      this.checkRowsCols();
      this.checkDiagonals(x, y);

      this.$forceUpdate();
    },

    checkDiagonals(x, y) {
      // Diagonal
      if (x === y) {
        this.diagonal.push(this.board[x][y]);
        if (
          this.areEqual(this.diagonal) &&
          this.diagonal.length === this.board.length
        ) {
          console.log('Diagonal Win');
        }
      }

      //Reverse Diagonal
      if (this.board.length - 1 - x === y) {
        this.reverseDiagonal.push(this.board[x][y]);
        if (
          this.areEqual(this.reverseDiagonal) &&
          this.reverseDiagonal.length === this.board.length
        ) {
          console.log('Reverse Diagonal Win');
        }
      }
    },

    checkRowsCols() {
      const transposedBoard = this.transpose(this.board);

      for (let i = 0; i < this.board.length; i++) {
        // Vertical Check
        if (this.areEqual(this.board[i])) {
          console.log('vertical Win');
        }

        // Horizontal Check
        if (this.areEqual(transposedBoard[i])) {
          console.log('Horizontal Win!');
        }
      }
    },
    transpose(matrix) {
      return matrix[0].map((_, c) => matrix.map(r => r[c]));
    },

    areEqual(arr) {
      if (arr.every(value => value === arr[0] && value !== '')) {
        return true;
      } else {
        return false;
      }
    }
  }
};
</script>
