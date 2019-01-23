<template>
  <div class="container">

    <h1 v-if="!lose && !win">{{ message }}!</h1>
    <h1 v-if="win">Winner!</h1>
    <h1 v-if="lose">Loser!</h1>

    <div class="dice-box">
      <div class="die">{{ die1 }}</div>
      <div class="die">{{ die2 }}</div>
    </div>

    <div class="col-sm">
      <div class="col-sm-12 lose-box" v-if="isLose">{{ message }}</div>
      <div class="col-sm-12 win-box" v-if="isWin">{{ message }}</div>
    </div>

    <div class="roll-total" v-if="rollTotal">
      <h4 class="roll-total" v-if="stickCall">"{{ stickCall }}"</h4> 
    </div>

    <div class="col-sm" v-if="!comeOut && !lose && !win" style="font-size: 2rem">Your point is {{ point }}!</div>

    <div class="col-sm">
      <button class="roll-button" @click.prevent="rollDice('http://soundbible.com/grab.php?id=182&type=mp3')">Roll</button>
    </div>

    <div class="col-sm">
      <button class="reset-button" @click="reset">Reset</button>
    </div>

    <div class="bet-box">
      <button @click="decreaseBet"><font-awesome-icon icon="minus" /></button>
      <button @click="increaseBet"><font-awesome-icon icon="plus" /></button>

      <div id="meterBet" :style="{width: betBox + 'px'}" style="background-color: black; color: white; text-align: center; line-height: 40px; 	margin: 10px;">{{ bet }}</div>
		  <div id="meterBet" :style="{width: bankBox + 'px'}" style="background-color: green; color: black; text-align: center; line-height: 40px; 	margin: 10px;">{{ bank }}</div>
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
      message: 'Roll em!',
      bank: 475,
      bet: 25,
    }
  },
  methods: {
    
    rollDice(sound) {
      
      if(sound) {
        var audio = new Audio(sound);
        audio.play();
      }

      this.die1=null;
      this.die2=null;
      let self = this;

      setTimeout(function() {
      
        self.lose = false;
        self.win = false;
        self.message = "Dice out!"
        
        self.die1 = Math.floor(Math.random() * 6) + 1;
        self.die2 = Math.floor(Math.random() * 6) + 1;
        self.rollTotal = self.die1 + self.die2;

        if (self.comeOut) {
          if (self.rollTotal === 2 || self.rollTotal === 3 || self.rollTotal === 12) {
            self.loseLogic("Craps!");
          } else if (self.rollTotal === 7 || self.rollTotal === 11) {
            self.winLogic("7/11 front-line winner!");
          } else {
            self.point = self.rollTotal;
            self.comeOut = false;
          }
        }

        else if (self.rollTotal === 7) {
          self.loseLogic("7 out!");
        }

        else if (self.rollTotal === self.point) {
          self.winLogic(self.point + "! Player hits their " + self.point + "!");
        }
      }, 400);
    },
    
    loseLogic(msg) {
      this.lose = true;
      this.point = null;
      this.comeOut = true;
      this.message = msg;
      this.bank -= this.bet;
    },

    winLogic(msg) {
      this.win = true;
      this.point = null;
      this.comeOut = true;
      this.message = msg;
      this.bank += this.bet;
    },
    reset() {
      this.win = false;
      this.lose = false;
      this.point = null;
      this.comeOut = true;
      this.die1 = null;
      this.die2 = null;
      this.message = "Come out roll!";
    },
    increaseBet(){
			if (this.bank > 0) {
          var audio = new Audio('http://soundbible.com/grab.php?id=2204&type=mp3');
          audio.play();
			  	this.bank -=25
				  this.bet +=25
			} 
			
		},
		decreaseBet(){
			if (this.bet > 0) {
				this.bank +=25
				this.bet -= 25
			} 
			
		}
  },
  computed: {
    isLose() {
      return this.lose;
    },
    isWin() {
      return this.win;
    },
    // eslint-disable-next-line
    stickCall() {
      if (this.rollTotal === 2) {
        return "snake eyes!"
      } else if (this.rollTotal === 3) {
        return "3 craps, 3 craps!"
      } else if (this.rolltotal === 4) {
        if (this.die1 === this.die2) {
          return "4 the hard way!"
        } else {
          return "4, easy 4!"
        }
      } else if (this.rollTotal === 5) {
        return "5, no field 5 "
      } else if (this.rollTotal === 6) {
        if (this.die1 === this.die2) {
          return "Hard 6, no field 6";
        } else {
          return "6 easy 6, no field 6";
        }
      } else if (this.rollTotal === 7) {
        if (this.win) {
          return "7 Winner, frontline winner 7! Take the dont's and field, pay the line"
        }
      } else if (this.rollTotal === 8) {
        if (this.die1===this.die2) {
          return "Hard 8, 44 We need you, 8 the hard way!"
        } else {
          return "8 easy 8, no field 8"
        }
      } else if (this.rollTotal === 9) {
        return "9 centerfield 9!"
      } else if (this.rollTotal === 10) {
        if (this.die1 === this.die2) {
          return "Hard 10, the ladies delight!"
        } else {
          return "10 easy 10, field roll 10"
        }
      } else if (this.rollTotal === 11) {
        if (this.win) {
          return "Yo 11! Take the don't come"
        } else {
          return "Yo 11! Pay the field and come."
        }
      } else if (this.rollTotal === 12) {
        if (this.die1 === this.die2) {
          return "12 craps 12, Bar the don't"
        } else {
          return "12 Box Cars!";
        }
      }
    },
    bankBox() {
      if(this.bank < 1500) {
        return this.bank * .6;
      } else {
        return this.bank * .2;
      }
        
    },
    betBox() {
     if(this.bet < 1500) {
        return this.bet * .6;
      } else {
        return this.bet * .2;
      }
    }}
}
</script>

<style>

  .roll-button {
    height: 4rem;
    width: 8rem;
    border-radius: 10px;
    border: 1px solid black;
    background-color: greenyellow;
    margin: .5rem;
    margin-left: auto;
    margin-right: auto;
  }

  .reset-button {
    height: 3rem;
    width: 6rem;
    border-radius: 10px;
    border: 1px solid black;
    background-color: hotpink;
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

  .bet-box {
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
    font-size: 2rem;
    width: 12rem;
    height: 4rem;
    border: 2px solid black;
    border-radius: 5px;
    background-color: red;
  }

  .win-box {
    font-size: 2rem;
    width: 12rem;
    height: 4rem;
    border: 2px solid black;
    border-radius: 5px;
    background-color: green;
  }

  #meterBank {
    width: 80%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    border-radius: 10px;
    transition: width 500ms;
  }

  #meterBet {
    width: 80%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    border-radius: 10px;
    transition: width 500ms;
  }

</style>


