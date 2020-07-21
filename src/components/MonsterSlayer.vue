<template>
  <div class="monster-slayer">
    <div class="players">
      <div class="one">
        <h1>YOU</h1>
        <div class="abs">
          <div :style="{width: widthOne + '%'}"  class="green-one">{{ widthOne }}</div>
        </div>
      </div>
      <div class="two">
        <h1>MONSTER</h1>
        <div class="abs">
          <div :style="{width: widthTwo + '%'}" class="green-two">{{ widthTwo }}</div>
        </div>
      </div>
    </div>

    <div class="start-new">
      <button @click="startGame" v-if="!started" class="start">START NEW GAME</button>
      <div v-else>
        <button @click="attack" class="attack">ATTACK</button>
        <button @click="special"  class="special">SPECIAL ATTACK</button>
        <button @click="heal" class="heal">HEAL</button>
        <button @click="giveUp" class="give-up">GIVE UP</button>
      </div>
    </div>

    <div v-if="displayResult" class="result">
      <div v-for="(values, index) in playerResult" :key="index">
        <div class="red">
          MONSTER HITS PLAYER FOR {{ playerResult[index] }}
        </div>
        <div class="blue" >
          PLAYER HITS MONSTER FOR {{ monsterResult[index] }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MonsterSlayer',
  props: {
    msg: String,
  },
  data() {
    return {
      started: false,
      widthOne: 100,
      widthTwo: 100,
      displayResult: false,
      monsterResult: [],
      playerResult: [],
      reducePlayer: null,
      reduceMonster: null,
    };
  },
  methods: {
    startGame() {
      this.started = !this.started;
      this.widthOne = 100;
      this.widthTwo = 100;
    },
    chakra(reducePlayer, reduceMonster) {
      this.reducePlayer = reducePlayer;
      this.reduceMonster = reduceMonster;
    },
    gameOver() {
      this.displayResult = false;
      this.started = false;
      this.widthOne = 100;
      this.widthTwo = 100;
    },
    attack() {
      this.chakra(Math.ceil(Math.random() * 10), Math.ceil(Math.random() * 10));
      this.playerResult.splice(0, 0, this.reducePlayer);
      this.widthOne -= this.reducePlayer;
      this.monsterResult.splice(0, 0, this.reduceMonster);
      this.widthTwo -= this.reduceMonster;
      this.displayResult = true;
    },
    special() {
      this.displayResult = true;
      const specialHitPlayer = Math.ceil(Math.random() * 30);
      const specialHitMonster = Math.ceil(Math.random() * 30);
      this.widthOne -= specialHitPlayer;
      this.widthTwo -= specialHitMonster;
      this.chakra(specialHitPlayer, specialHitMonster);
      this.playerResult.splice(0, 0, this.reducePlayer);
      this.monsterResult.splice(0, 0, this.reduceMonster);
    },
    heal() {
      if (this.widthOne < 100) {
        const heal = Math.ceil(Math.random() * 9);
        this.widthOne += heal;
        console.log('here');
      } else {
        this.widthOne = 100;
      }
    },
    giveUp() {
      this.gameOver();
      alert('You gave up. Coward!');
    },
  },
  updated() {
    if (this.widthOne > 100) {
      this.widthOne = 100;
    }
    if (this.widthOne < 0) {
      this.widthOne = 0;
      this.gameOver();
      alert('You lost to Monster!');
    }
    if (this.widthTwo < 0) {
      this.widthTwo = 0;
      this.gameOver();
      alert('You beat Monster!');
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import 'mixins';

div.monster-slayer {
  text-align: center;
  width: 80%;
  margin: 0 auto;
  .players {
    @include flex(space-between, left);
    width: 75%;
    margin: 0 auto;
    font-family: Arial, Helvetica, sans-serif;
    .abs {
      width: 100%;
      height: 40px;
      background: rgb(236, 236, 236);
      position: absolute;
      top: 70px;
      z-index: 1;
    }
    .one, .two {
      position: relative;
      margin: 0 auto;
      width: 350px;
      height: 160px;
    }
    .green-one {
      @include flex(center, center);
      @include playerDiv(40px, rgb(10, 168, 10), 2, #fff);
      transition: .7s;
    }
    .green-two {
      @include flex(center, center);
      @include playerDiv(40px, rgb(10, 168, 10), 2, #fff);
      transition: .7s;
    }
  }
  .start-new {
    width: 100%;
    height: 90px;
    border-radius: 4px;
    box-shadow: 0px 4px 10px rgba(0,0,0,0.2);
    border: 1px solid #ccc;
    @include flex(center, center);
    margin-bottom: 40px;
    .start {
      @include button(200px, rgb(35, 201, 35), #fff, rgb(16, 167, 16));
    }
    div {
      width: 500px;
      @include flex(space-between, center);
      .attack {
        @include button(100px, rgb(241, 124, 134), #000, rgb(201, 74, 84));
      }
      .special {
        @include button(150px, rgb(219, 200, 91), #000, rgb(190, 170, 52));
      }
      .heal {
        @include button(100px, rgb(35, 201, 35), #000, rgb(14, 163, 14));
      }
      .give-up {
        @include button(100px,#fff, #000, rgb(219, 219, 219));
      }
    }
  }
  .result {
    width: 100%;
    height: auto;
    border-radius: 4px;
    box-shadow: 0px 4px 10px rgba(0,0,0,0.2);
    border: 1px solid #ccc;
    @include flex(center, center);
    padding: 15px;
    box-sizing: border-box;
    display: block;
    .red {
      background: rgb(241, 124, 134);
      padding: 5px 0;
      color: rgb(71, 7, 13);
      margin-bottom: 10px;
    }
    .blue {
      background: rgb(124, 198, 241);
      padding: 5px 0;
      color: rgb(26, 86, 121);
      margin-bottom: 10px;
  }
  }
}
</style>
