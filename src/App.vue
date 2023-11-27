<script>
export default {
  data() {
    return {
      newGameData: false,
      newGame: false,
      gameStarted: false,
      matchEnded: false,
      player1Color: "",
      player2Color: "",
      player1Name: "",
      player2Name: "",
      set: 0,
      point: 0,
      server: -1,
      position: -1,
      lastPlayer: -1,

      match: {
        player1: {
          name: "Player 1",
          color: "red",
          sets: 0,
        },
        player2: {
          name: "Player 2",
          color: "blue",
          sets: 0,
        },
        sets: [[[0, 0]], [[0, 0]], [[0, 0]], [[0, 0]], [[0, 0]]],
      },
    };
  },
  methods: {
    setPlayer1Color(color) {
      this.player1Color = color;
    },
    setPlayer2Color(color) {
      this.player2Color = color;
    },
    setPlayers() {
      if (
        this.player1Name &&
        this.player2Name &&
        this.player1Color &&
        this.player2Color
      ) {
        this.match = {
          player1: {
            name: this.player1Name,
            color: this.player1Color,
            sets: 0,
          },
          player2: {
            name: this.player2Name,
            color: this.player2Color,
            sets: 0,
          },
          sets: [[[0, 0]], [[0, 0]], [[0, 0]], [[0, 0]], [[0, 0]]],
        };
        this.gameStarted = true;
        this.newGameData = false;
        this.newGame = false;
      }
    },
    newPoint(player) {
      if (player === 0) {
        this.match.sets[this.set].push([
          this.match.sets[this.set][this.point][0] + 1,
          this.match.sets[this.set][this.point][1],
        ]);
        if (this.lastPlayer != 0) {
          this.position = -1;
          this.lastPlayer = 0;
          this.server = 0;
        } else {
          if (this.position == 0) {
            this.position = 1;
          } else {
            this.position = 0;
          }
        }
        this.point++;
        if (
          this.match.sets[this.set][this.point][0] >= 11 &&
          this.match.sets[this.set][this.point][0] -
            this.match.sets[this.set][this.point][1] >=
            2
        ) {
          this.match.player1.sets++;
          this.set++;
          this.point = 0;
          this.position = -1;
          if (this.match.player1.sets == 3 || this.match.player2.sets == 3) {
            this.matchEnded = true;
            console.log(this.match);
          }
        }
      } else {
        this.match.sets[this.set].push([
          this.match.sets[this.set][this.point][0],
          this.match.sets[this.set][this.point][1] + 1,
        ]);
        if (this.lastPlayer != 1) {
          this.position = -1;
          this.lastPlayer = 1;
          this.server = 1;
        } else {
          if (this.position == 0) {
            this.position = 1;
          } else {
            this.position = 0;
          }
        }
        this.point++;
        if (
          this.match.sets[this.set][this.point][1] >= 11 &&
          this.match.sets[this.set][this.point][1] -
            this.match.sets[this.set][this.point][0] >=
            2
        ) {
          this.match.player2.sets++;
          this.set++;
          this.point = 0;
          this.position = -1;
          if (this.match.player1.sets == 3 || this.match.player2.sets == 3) {
            this.matchEnded = true;
            console.log(this.match);
          }
        }
      }
    },
    startNewGame() {
      window.location.reload();
    },
  },
};
</script>

<template>
  <button v-if="!newGameData && !gameStarted" @click="newGameData = true">
    New Game
  </button>
  <div v-if="newGameData">
    <h1>Enter player names and colors</h1>
    <h2>Player 1</h2>
    <input type="text" name="player1" v-model="player1Name" />
    <div class="colors">
      <button
        class="color-button"
        @click="setPlayer1Color('#ff7f7f')"
        :class="{ selected: player1Color === '#ff7f7f' }"
      >
        <span class="color" style="background-color: #ff7f7f"></span>
      </button>
      <button
        class="color-button"
        @click="setPlayer1Color('#7f7fff')"
        :class="{ selected: player1Color === '#7f7fff' }"
      >
        <span class="color" style="background-color: #7f7fff"></span>
      </button>
      <button
        class="color-button"
        @click="setPlayer1Color('#7fff7f')"
        :class="{ selected: player1Color === '#7fff7f' }"
      >
        <span class="color" style="background-color: #7fff7f"></span>
      </button>
      <button
        class="color-button"
        @click="setPlayer1Color('#ff7fff')"
        :class="{ selected: player1Color === '#ff7fff' }"
      >
        <span class="color" style="background-color: #ff7fff"></span>
      </button>
      <button
        class="color-button"
        @click="setPlayer1Color('#fff')"
        :class="{ selected: player1Color === '#fff' }"
      >
        <span class="color" style="background-color: #fff"></span>
      </button>
    </div>
    <h2>Player 2</h2>
    <input type="text" name="player2" v-model="player2Name" />
    <div class="colors">
      <button
        class="color-button"
        @click="setPlayer2Color('#c0dbf0')"
        :class="{ selected: player2Color === '#c0dbf0' }"
      >
        <span class="color" style="background-color: #c0dbf0"></span>
      </button>
      <button
        class="color-button"
        @click="setPlayer2Color('#c6fcfc')"
        :class="{ selected: player2Color === '#c6fcfc' }"
      >
        <span class="color" style="background-color: #c6fcfc"></span>
      </button>
      <button
        class="color-button"
        @click="setPlayer2Color('#eee0cf')"
        :class="{ selected: player2Color === '#eee0cf' }"
      >
        <span class="color" style="background-color: #eee0cf"></span>
      </button>
      <button
        class="color-button"
        @click="setPlayer2Color('#f0d2a6')"
        :class="{ selected: player2Color === '#f0d2a6' }"
      >
        <span class="color" style="background-color: #f0d2a6"></span>
      </button>
      <button
        class="color-button"
        @click="setPlayer2Color('#fcd1bf')"
        :class="{ selected: player2Color === '#fcd1bf' }"
      >
        <span class="color" style="background-color: #fcd1bf"></span>
      </button>
    </div>
    <button @click="setPlayers()">Start</button>
  </div>
  <div class="serverPopup" v-if="server == -1 && gameStarted">
    <div class="serverPopupContent">
      <h1>Who is serving?</h1>
      <button @click="server = lastPlayer = 0">{{ match.player1.name }}</button>
      <button @click="server = lastPlayer = 1">{{ match.player2.name }}</button>
    </div>
  </div>
  <div class="sidePopup" v-if="position == -1 && server != -1 && gameStarted">
    <div class="serverPopupContent">
      <h1 v-if="server == 0">Where is {{ match.player1.name }} serving ?</h1>
      <h1 v-if="server == 1">Where is {{ match.player2.name }} serving ?</h1>
      <button @click="position = 1">Left</button>
      <button @click="position = 0">Right</button>
    </div>
  </div>
  <div v-if="gameStarted">
    <h1>{{ match.player1.name }} vs {{ match.player2.name }}</h1>
    <h2>Set {{ set + 1 }}</h2>

    <div class="buttons">
      <button @click="newPoint(0)">{{ match.player1.name }}</button>
      <button @click="newPoint(1)">{{ match.player2.name }}</button>
    </div>
    <div
      class="ball-position"
      :class="{ left: position == 1, right: position == 0 }"
    >
      <span
        v-if="server == 0"
        class="ball"
        :style="'background-color:' + match.player1.color"
      ></span>
      <span
        v-if="server == 1"
        class="ball"
        :style="'background-color:' + match.player2.color"
      ></span>
    </div>
    <div class="players-data">
      <div class="player-1">
        <p>{{ match.player1.name }}</p>
        <span :style="'background-color:' + match.player1.color"></span>
      </div>
      <div class="player-1-sets">
        <p>{{ match.player1.sets }}</p>
      </div>
      <div class="score">
        <p>{{ match.sets[set][point][0] }} - {{ match.sets[set][point][1] }}</p>
      </div>
      <div class="player-2-sets">
        <p>{{ match.player2.sets }}</p>
      </div>
      <div class="player-2">
        <p>{{ match.player2.name }}</p>
        <span :style="'background-color:' + match.player2.color"></span>
      </div>
    </div>
  </div>
  <div v-if="matchEnded" class="game-end">
    <h1>Match ended</h1>
    <h2>{{ match.player1.name }} vs {{ match.player2.name }}</h2>
    <h2 class="final-score">
      {{ match.player1.sets }} - {{ match.player2.sets }}
    </h2>
    <div class="sets" v-for="setRev in match.sets">
      <p v-if="setRev.length > 1">
        {{ setRev[setRev.length - 1][0] }} - {{ setRev[setRev.length - 1][1] }}
      </p>
    </div>
    <button @click="startNewGame">New Game</button>
  </div>
</template>

<style scoped>
h1 {
  margin: 0;
  padding: 0;
  font-size: 1.5rem;
  font-weight: 400;
  margin-bottom: 1rem;
}
h2 {
  margin: 0;
  padding: 0;
  font-size: 1rem;
  font-weight: 400;
  margin-bottom: 0.5rem;
}
input {
  width: 100%;
  margin-bottom: 1rem;
  text-align: center;
  padding: 0.5rem 0;
}
button.selected {
  background-color: #ebebeb4e;
}
.color-button {
  margin: 0;
  padding: 0;
  width: 30px;
  height: 30px;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  border-radius: 10rem;
  background-color: transparent;
}
.colors {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding: 1rem 0;
}
.color {
  width: 20px;
  height: 20px;
  border-radius: 10rem;
  display: block;
}
.serverPopup,
.sidePopup {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100%;
  background-color: #242424;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}
.serverPopup button,
.sidePopup button,
.buttons button {
  margin: 0 0.5rem;
}

.players-data {
  position: fixed;
  bottom: 0;
  left: 0;
  height: 100px;
  border-top: #ebebeb4e 1px solid;
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}
.players-data span {
  width: 70%;
  height: 5px;
  display: inline-block;
  margin-left: 0.5rem;
}
.score {
  width: 30%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}
.player-1-sets,
.player-2-sets {
  width: 10%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}
.final-score {
  font-size: 2rem;
  margin-bottom: 1rem;
}
.player-1,
.player-2 {
  width: 25%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.set-line {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}
.set-line p {
  margin: 0;
}
.ball-position {
  margin-top: 2rem;
  width: 100%;
  display: flex;
  flex-direction: row;
}
.left {
  justify-content: flex-start;
}

.right {
  justify-content: flex-end;
}
.ball {
  width: 30px;
  height: 30px;
  border-radius: 10rem;
  display: block;
  background-color: green;
}
.game-end {
  position: fixed;
  z-index: 100;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100%;
  background-color: #242424;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
