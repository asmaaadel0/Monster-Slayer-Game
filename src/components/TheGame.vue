<template>
  <body>
    <header>
      <h1>Monster Slayer</h1>
    </header>
    <div id="game">
      <section id="monster" class="container">
        <h2>Monster Health</h2>
        <div class="healthbar">
          <div class="healthbar__value" :style="monsterBarStyles"></div>
        </div>
      </section>
      <section id="player" class="container">
        <h2>Your Health</h2>
        <div class="healthbar">
          <div class="healthbar__value" :style="playerBarStyles"></div>
        </div>
      </section>
      <section class="container" v-if="winner || firstPlay">
        <h2 v-if="winner">Game Over!</h2>
        <h3 v-if="winner==='monster'">You Lost!</h3>
        <h3 v-else-if="winner==='player'">You Won</h3>
        <h3 v-else-if="winner==='draw'">It's a Draw</h3>
        <h3 v-else></h3>
        <button @click="startGame()">Start New Game</button>
      </section >
      <section id="controls" v-else>
        <button @click="attackMonster()">ATTACK</button>
        <button @click="specialAttackMonster()" :disabled="mayUseSpecialAttack">
          SPECIAL ATTACK
        </button>
        <button @click="healPlayer()">HEAL</button>
        <button @click="surrender()">GIVE UP</button>
      </section>
      <section id="log" class="container">
        <h2>Battle Log</h2>
        <ul>
          <li v-for="message in messagesLog" :key="message">
            <span
              :class="{'log--player': message.actionBy === 'player' , 'log--monster':message.actionBy === 'monster'}"
              >{{message.actionBy === 'player' ? 'Player' : 'Monster'}}
            </span>
            <span v-if="message.actionType === 'heal'"
              >heals himself for
              <span class="log--heal">{{message.actionValue}}</span></span
            >
            <span v-else>
              attacks and deals
              <span class="log--damage">{{ message.actionValue }}</span>
            </span>
          </li>
        </ul>
      </section>
    </div>
  </body>

</template>

<script>
  function getRandomValue(min, max) {
  return Math.floor(Math.random() * (max - min)) + min;
}
export default {
  data() {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      currentRound: 0,
      winner: null,
      messagesLog: [],
      firstPlay:true,
    };
  },
  watch: {
    playerHealth(value) {
      if (value <= 0 && this.monsterHealth <= 0) {
        this.winner = "draw";
      } else if (value <= 0) {
        this.winner = "monster";
      }
    },
    monsterHealth(value) {
      if (value <= 0 && this.playerHealth <= 0) {
        this.winner = "draw";
      } else if (value <= 0) {
        this.winner = "player";
      }
    },
  },
  computed: {
    monsterBarStyles() {
      if (this.monsterHealth < 0) {
        return { width: "0%" };
      }
      return { width: this.monsterHealth + "%" };
    },
    playerBarStyles() {
      if (this.playerHealth < 0) {
        return { width: "0%" };
      }
      return { width: this.playerHealth + "%" };
    },
    mayUseSpecialAttack() {
      return this.currentRound % 3 !== 0;
    },
  },
  methods: {
    startGame() {
      this.playerHealth = 100;
      this.monsterHealth = 100;
      this.currentRound = 0;
      this.winner = null;
      this.messagesLog = [];
      this.firstPlay=false;
    },
    attackMonster() {
      this.currentRound++;
      const attackValue = getRandomValue(5, 12);
      this.monsterHealth -= attackValue;
      this.addLogMessage('player','attack',attackValue);
      this.attackPalyer();
    },
    attackPalyer() {
      const attackValue = getRandomValue(8, 15);
      this.playerHealth -= attackValue;
      this.addLogMessage('monster','attack',attackValue);
    },
    specialAttackMonster() {
      this.currentRound++;
      const attackValue = getRandomValue(10, 25);
      this.monsterHealth -= attackValue;
      this.addLogMessage('player','attack',attackValue);
      this.attackPalyer();
    },
    healPlayer() {
      this.currentRound++;
      const healValue = getRandomValue(10, 20);
      if (this.playerHealth + healValue > 100) this.playerHealth = 100;
      else this.playerHealth += healValue;
      this.addLogMessage('player','heal',healValue);
      this.attackPalyer();
    },
    surrender() {
      this.winner = "monster";
    },
    addLogMessage(who, what, value) {
      this.messagesLog.unshift({
        actionBy: who,
        actionType: what,
        actionValue: value,
      });
    },
  },
}
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: 'Jost', sans-serif;
}

body {
  margin: 0;
}

header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 0.5rem;
  background-color: #dd169b;
  color: white;
  text-align: center;
  margin-bottom: 2rem;
}

section {
  width: 90%;
  max-width: 40rem;
  margin: auto;
}

.healthbar {
  width: 100%;
  height: 40px;
  border: 1px solid #575757;
  margin: 1rem 0;
  background: #fde5e5;
}

.healthbar__value {
  background-color: #d1ff04;
  width: 100%;
  height: 100%;
}

.container {
  text-align: center;
  padding: 0.5rem;
  margin: 1rem auto;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  border-radius: 1rem;
}

#monster h2,
#player h2 {
  margin: 0.25rem;
}

#controls {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
}

button {
  font: inherit;
  border: 1px solid #dd169b;
  background-color: #dd169b;
  color: white;
  padding: 1rem 2rem;
  border-radius: 1rem;
  margin: 1rem;
  width: 12rem;
  cursor: pointer;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
}

button:focus {
  outline: none;
}

button:hover,
button:active {
  background-color: #ea2dab;
  border-color: #ea2dab;
  box-shadow: 1px 1px 8px rgba(0, 0, 0, 0.26);
}

button:disabled {
  background-color: #ccc;
  border-color: #ccc;
  box-shadow: none;
  color: #3f3f3f;
  cursor: not-allowed;
}

#log ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

#log li {
  margin: 0.5rem 0;
}

.log--player {
  color: #7700ff;
}

.log--monster {
  color: #da8d00;
}

.log--damage {
  color: red;
}

.log--heal {
  color: green;
}
</style>