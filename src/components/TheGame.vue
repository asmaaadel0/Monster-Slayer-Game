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
export default {

}
</script>

<style>

</style>