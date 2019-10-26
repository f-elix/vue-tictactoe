<template>
  <div id="app">
    <transition appear>
      <end-game-modal v-if="modalOpened" @close-modal="modalOpened = false">{{
        endGameMsg
      }}</end-game-modal>
    </transition>
    <div class="game">
      <button @click="gameStarted = true" v-if="!gameStarted">
        Start Game
      </button>
      <button @click="reset" v-else class="reset">Reset</button>
      <div class="turns">
        <div
          class="turns__item"
          :class="{ 'turns__item--enabled': turn === 'x' && gameStarted }"
        >
          <div class="x"></div>
        </div>
        <div
          class="turns__item"
          :class="{ 'turns__item--enabled': turn === 'o' && gameStarted }"
        >
          <div class="o"></div>
        </div>
      </div>
      <div class="board">
        <div
          class="board__square"
          v-for="(square, i) in squares"
          :key="i"
          @click="play(i)"
          :class="{ 'board__square--win': square.win }"
        >
          <div class="x" v-if="square.x"></div>
          <div class="o" v-else-if="square.o"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import EndGameModal from "./components/EndGameModal.vue";

export default {
  components: {
    "end-game-modal": EndGameModal
  },
  data() {
    return {
      squaresArray: [],
      turn: "x",
      gameStarted: false,
      endGameMsg: "",
      modalOpened: false
    };
  },
  computed: {
    squares() {
      const squares = this.squaresArray;
      for (let i = 0; i < 9; i++) {
        const square = {
          o: false,
          x: false,
          win: false,
          filled: false
        };
        squares.push(square);
      }
      return squares;
    }
  },
  methods: {
    play(i) {
      if (this.gameStarted && !this.squares[i].filled) {
        this.squares[i][this.turn] = true;
        this.squares[i].filled = true;
        this.turn = this.turn === "x" ? "o" : "x";
        this.checkWin("x");
        this.checkWin("o");
      }
    },
    showWin(squareIndexes, result) {
      squareIndexes.forEach(index => {
        this.squares[index].win = true;
      });
      this.endGameMsg = result === "draw" ? "Draw" : `${result} wins!`;
      this.modalOpened = true;
    },
    checkWin(item) {
      // Check rows
      for (let i = 0; i < 6; i += 3) {
        if (
          this.squares[i][item] &&
          this.squares[i + 1][item] &&
          this.squares[i + 2][item]
        ) {
          this.showWin([i, i + 1, i + 2], item);
          return;
        }
      }
      // Check columns
      for (let i = 0; i < 2; i++) {
        if (
          this.squares[i][item] &&
          this.squares[i + 3][item] &&
          this.squares[i + 6][item]
        ) {
          this.showWin([i, i + 3, i + 6], item);
          return;
        }
      }
      // Check diagonals
      if (
        this.squares[0][item] &&
        this.squares[4][item] &&
        this.squares[8][item]
      ) {
        this.showWin([0, 4, 8], item);
        return;
      }
      if (
        this.squares[2][item] &&
        this.squares[4][item] &&
        this.squares[6][item]
      ) {
        this.showWin([2, 4, 6], item);
        return;
      }
      // Draw
      const draw = this.squares.every(square => {
        return square.filled === true;
      });
      if (draw) {
        this.showWin([], "draw");
        return;
      }
      // Game not finished
      return;
    },
    reset() {
      this.gameStarted = false;
      this.filledSquares = 0;
      this.squares.forEach(square => {
        square.o = false;
        square.x = false;
        square.win = false;
        square.filled = false;
      });
    }
  }
};
</script>

<style>
:root {
  --white: #e3e3e3;
  --xo-color: var(--white);
}

body {
  margin: 0;
}

#app {
  height: 100vh;
  background-color: #212121;
  color: var(--white);
  overflow: auto;
}

.game {
  margin: 3rem auto;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.game button {
  display: block;
  margin: 0 auto;
  padding: 0.75rem;
  color: var(--white);
  background-color: #3f6939;
  font-weight: bold;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.game button.reset {
  background-color: #a02f2f;
}

.turns {
  width: 100%;
  max-width: 30rem;
  padding: 1.5rem;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.turns__item {
  padding: 0.75rem;
  width: 6rem;
  height: 6rem;
  background-color: #696969;
  border-radius: 5px;
  opacity: 0.4;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 0.15s ease-out;
}

.turns__item--enabled {
  opacity: 1;
}

.turns__item--enabled .x,
.turns__item--enabled .o {
  --xo-color: lawngreen;
}

.board {
  margin-top: 3rem;
  width: 100%;
  /* height: 21rem; */
  max-width: 30rem;
  height: 30rem;
  background-color: var(--white);
  display: grid;
  grid-template-rows: repeat(3, 1fr);
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 2px;
  box-shadow: inset 0 0 50px #212121;
  overflow: hidden;
}

.board__square {
  background-color: #212121;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 0.2s ease;
}

.board__square--win {
  background-color: #3f6939;
}

.board__square:hover {
  background-color: #696969;
  box-shadow: 0 0 30px 15px #212121;
}

.x {
  position: relative;
}

.x::before,
.x::after {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 4.5rem;
  height: 5px;
  background-color: var(--xo-color);
  transform: translate(-50%, -50%) rotate(45deg);
}

.x::after {
  transform: translate(-50%, -50%) rotate(-45deg);
}

.o {
  width: 4.5rem;
  height: 4.5rem;
  border-radius: 50%;
  border: 5px solid var(--xo-color);
}

.v-enter {
  transform: scale(0);
  opacity: 0;
}

.v-enter-active {
  transition: all ease 0.4s;
}
</style>
