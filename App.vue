<template>
  <div id="app">
    <section class="row">
      <div class="small-6 columns">
        <h1 class="text-center">PLAYER</h1>
        <div class="wins">
          <div class="wins text-center" style="background-color: green; margin: 0; color: white;" :style="{width: playerWins + '0%'}">
            {{playerWins}}{{playerPoints()}}

          </div>
        </div>
      </div>
      <div class="small-6 columns">
        <h1 class="text-center">COMPUTER</h1>
        <div class="wins">
          <div class="wins text-center" style="background-color: green; margin: 0; color: white;" :style="{width: computerWins + '0%'}">
            {{computerWins}}{{computerPoints()}}

          </div>
        </div>
      </div>
    </section>
    <section class="row controls" v-show="gameStart">
      <div class="small-12 columns">
        <button id="start-game" v-on:click="gameStart = !gameStart">START NEW GAME</button>
      </div>


    </section>
    <section class="row controls">
      Bet Amount: {{bets}}
      <button @click="bets++, playerBets--">Bets Available: {{playerBets}}</button>
      Rounds: {{round}} <div>Your Score: {{score}} <br> HighScore: {{highScore}}</div>

    </section>
    <div>{{emptyBet()}}  {{rounds()}} {{totalScore}}</div>


    <section class="row controls" v-show="!gameStart">
      <div class="small-12 columns">
        <button @click="rock" :class="player" id="rock">ROCK</button>
        <button @click="paper" :class="player" id="paper">PAPER</button>
        <button @click="scissors" :class="player" id="scissors">SCISSORS</button>
        <button @click="restart" :class="player" id="restart">RESTART</button>
      </div>
    </section>
    <section class="row log">
      <div class="small-12 columns">
        <ul>
          <li v-for="result in turns" :class="{'player-turn' : result.won, 'computer-turn' : result.lose, 'tie-turn': result.tied}">
            {{result.text}}
          </li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      gameStart: true,
      player: {rock: false, paper: false, scissors: false},
      playerWon: {won: false, lose: false, tied: false},
      computerWins: 0,
      playerWins: 0,
      turns: [],
      playerBets: 15,
      score: 0,
      highScore: 0,
      round: 0,
      bets: 0

    }
  },
  methods: {
    rock: function () {
      var computer = this.randomComputer();
      console.log(computer);
      if (computer === 2) {
        this.turns.unshift({
          lose: true,
          text: 'Computer chose Paper | Paper beats Rock | Computer Wins!'
        });
        this.computerWins += 1;
        this.bets = this.bets - this.bets;
        this.round++
      }
      else if (computer === 3) {
        this.turns.unshift({
          won: true,
          text: 'Computer chose Scissors | Rock beats Scissors | Player Wins!'
        });
        this.playerWins += 1;
        this.score += this.bets;
        this.playerBets = this.playerBets + (this.bets * 2);
        this.bets = 0;
        this.round++;
      }
      else {
        this.turns.unshift({
          tied: true,
          text: 'Computer chose Rock | Its a tie!'
        });
        this.round++;
        this.bets = 0;
      }
    },

    paper: function () {
      var computer = this.randomComputer();
      if (computer === 3) {
        this.turns.unshift({
          lose: true,
          text: 'Computer chose Scissors | Scissors beats Paper | Computer Wins!'
        });
        this.computerWins += 1;
        this.bets = this.bets - this.bets;
        this.round++
      }
      else if (computer === 1) {
        this.turns.unshift({
          won: true,
          text: 'Computer chose Rock | Paper beats Rock | Player Wins!'
        });
        this.playerWins += 1;
        this.score += this.bets;
        this.playerBets = this.playerBets + (this.bets * 2);
        this.bets = 0;
        this.round++;
      }
      else {
        this.turns.unshift({
          tied: true,
          text: 'Computer chose Paper | Its a tie!'
        });
        this.round++;
        this.bets = 0;
      }
    },

    scissors: function () {
      var computer = this.randomComputer();
      if (computer === 1) {
        this.turns.unshift({
          lose: true,
          text: 'Computer chose Rock | Rock beats Scissors | Computer Wins!'
        });
        this.computerWins += 1;
        this.bets = this.bets - this.bets;
        this.round++
      }
      else if (computer === 2) {
        this.turns.unshift({
          won: true,
          text: 'Computer chose Paper | Scissors beats Paper | Player Wins!'
        });
        this.playerWins += 1;
        this.score += this.bets;
        this.playerBets = this.playerBets + (this.bets * 2);
        this.bets = 0;
        this.round++;
      }
      else {
        this.turns.unshift({
          tied: true,
          text: 'Computer chose Scissors | Its a tie!'
        });
        this.round++;
        this.bets = 0;
      }
    },

    restart: function () {
      this.computerWins = 0;
      this.playerWins = 0;
      this.turns = [];
      this.show = true;
      if(this.score > this.highScore){
        this.highScore = this.score;
      }
      this.score = 0;
      this.playerBets = 15;
      this.round = 0;
      this.bets = 0;
    },

    randomComputer: function () {
      var choice = Math.floor((Math.random()*3) + 1);
      return choice;
    },

    computerPoints: function () {
      var points = this.computerWins;
      if (points === 10) {
        return alert('Game Over! Computer wins. Restart to play again.');
      }
    },

    playerPoints: function () {
      var points2 = this.playerWins;
      if (points2 === 10) {
        return alert('Game Over! You won. Restart to play again.');
      }
    },



    emptyBet: function () {
      var points = this.bets;
      var playerPoints = this.playerBets;
      if(points <= 0 && playerPoints <= 0){
        return alert("No more bets left. Game Over");
      }
    },

//    betStart: function () {
//      var points = this.bets;
//      if(points <= 0){
//        this.bets = 0;
//        return alert("Please enter your bet amount before playing");
//      }
//    },

    rounds: function(){
      var gameRounds = this.round;
      if(gameRounds === 9){
        return alert("Game Over. Max rounds played");
      }
    }
  },
  computed: {
    totalScore: function () {
      if (this.score > this.highScore){
        this.highScore = this.score;
        alert('You have set the new high score!')
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
.text-center {
  text-align: center;
}

.wins {
  width: 80%;
  color: black;
  height: 40px;
  background-color: #eee;
  margin: auto;
  transition: width 1000ms;
}

.controls, .log {
  margin-top: 30px;
  text-align: center;
  padding: 10px;
  border: 1px solid #ccc;
  box-shadow: 0px 3px 6px #ccc;
}

.turn {
  margin-top: 20px;
  margin-bottom: 20px;
  font-weight: bold;
  font-size: 22px;
}

.log ul {
  list-style: none;
  font-weight: bold;
  text-transform: uppercase;
}

.log ul li {
  margin: 5px;
}

.log ul .player-turn {
  color: green;
  background-color: #aaffb0;
}

.log ul .computer-turn {
  color: red;
  background-color: #ffc0c1;
}

.log ul .tie-turn {
  color: blue;
  background-color: #e4e8ff;
}

button {
  font-size: 20px;
  background-color: #eee;
  padding: 12px;
  box-shadow: 0 1px 1px black;
  margin: 10px;
}

#start-game {
  background-color: #e4e8ff;
}

#start-game:hover {
  background-color: #687eff;
}

#rock {
  background-color: #ff7367;
}

#rock:hover {
  background-color: #ff3f43;
}

#paper {
  background-color: #ffaf4f;
}

#paper:hover {
  background-color: #ff9a2b;
}

#scissors {
  background-color: #aaffb0;
}

#scissors:hover {
  background-color: #76ff7e;
}

#restart {
  background-color: #ffffff;
}

#restart:hover {
  background-color: #c7c7c7;
}
</style>
