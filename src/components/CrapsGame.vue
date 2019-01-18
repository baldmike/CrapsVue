<template>
  <div class="container">

    <h1>Craps!</h1>

    <div class="col-sm" v-if="!comeOut" style="font-size: 2rem">Your point is {{ point }}!</div>

    <div class="dice-box">
      <div class="die">{{ die1 }}</div>
      <div class="die">{{ die2 }}</div>
    </div>

    <div class="col-sm">
      <div class="col-sm-12 lose-box" v-if="isLose">You lose!</div>
      <div class="col-sm-12 win-box" v-if="isWin">You Win!</div>
    </div>

    <div class="col-sm">
      <button class="roll-button" @click="rollDice">Roll</button>
    </div>

    <div class="roll-total" v-if="rollTotal">
      {{ rollTotal }}
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      die1: 1,
      die2: 2,
      rollTotal: null,
      comeOut: true,
      lose: false,
      win: false,
      point: null,
    }
  },
  methods: {
    rollDice() {
      this.lose = false;
      this.win = false;
      
      const dieRoll1 = Math.floor(Math.random() * 6) + 1;
      const dieRoll2 = Math.floor(Math.random() * 6) + 1;
      this.rollTotal = dieRoll1 + dieRoll2;

      this.die1 = dieRoll1;
      this.die2 = dieRoll2;

      if (this.comeOut) {
        if (this.rollTotal === 2 || this.rollTotal === 3 || this.rollTotal === 12) {
          this.loseLogic();
        } else if (this.rollTotal === 7 || this.rollTotal === 11) {
          this.winLogic();
        } else {
          this.point = this.rollTotal;
          this.comeOut = false;
        }
      }

      else if (this.rollTotal === 7) {
        this.loseLogic();
      }

      else if (this.rollTotal === this.point) {
        this.winLogic();
      }

    },
    loseLogic() {
      this.lose = true;
      this.point = null;
      this.comeOut = true;

    },
    winLogic() {
      this.win = true;
      this.point = null;
      this.comeOut = true;
    }
  },
  computed: {
    isLose() {
      return this.lose;
    },
    isWin() {
      return this.win;
    }
  }
}
</script>

<style>

  .roll-button {
    height: 4rem;
    width: 6rem;
    border-radius: 10px;
    border: 1px solid black;
    background-color: greenyellow;
    margin: .5rem;
    margin-left: auto;
    margin-right: auto;
  }

  .roll-box {
    position: fixed;
    bottom: 0;
  }

  .dice-box {
    display: inline-block;
  }

  .die {
    height: 4rem;
    width: 4rem;
    border-radius: 5px;
    border: 1px solid black;
    background-color: red;
    color: white;
    font-size: 2rem;
    line-height: 3rem;
    margin: .5rem;
    display: inline-block;
  }

  .lose-box {
    width: 12rem;
    height: 4rem;
    border: 2px solid black;
    border-radius: 5px;
    background-color: red;
  }

  .win-box {
    width: 12rem;
    height: 4rem;
    border: 2px solid black;
    border-radius: 5px;
    background-color: green;
  }

</style>


