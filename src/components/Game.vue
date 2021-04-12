<template>
  <div class="container">
    <h1>Simon Says</h1>
    <div class="simon">
      <ul>
        <li class="red"
            data-tile="1"
            :class="{'redOn': one}"
            @click="firstClick"
        ></li>
        <li class="blue"
            data-tile="2"
            :class="{'blueOn': second}"
            @click="secondClick"
        ></li>
        <li class="yellow"
            data-tile="3"
            :class="{'yellowOn': third}"
            @click="thirdClick"
        ></li>
        <li class="green"
            data-tile="4"
            :class="{'greenOn': fourty}"
            @click="fourtyClick"
        ></li>
      </ul>
    </div>
    <div class="game-info">
      <h2>Раунд: <span data-round="0">{{round}}</span></h2>
      <button data-action="start" @click="startGame" :disabled="gameStart">Начать</button>
      <p data-action="lose"><span v-if="gameOver">Извините, но вы проиграли!</span></p>
    </div>
    <div class="game-options">
      <h2>Уровни сложности:</h2>
      <input type="radio" name="mode" value="1500" v-model="interval" :disabled="gameStart" checked="">Легкий<br>
      <input type="radio" name="mode" value="1000" v-model="interval" :disabled="gameStart">Средний<br>
      <input type="radio" name="mode" value="400" v-model="interval" :disabled="gameStart">Сложный
    </div>
    <div class="sound">
      <audio id="noise1">
        <source src="@/sounds/1.mp3" type="audio/mp3" />
      </audio>
      <audio id="noise2">
        <source src="@/sounds/2.mp3" type="audio/mp3" />
      </audio>
      <audio id="noise3">
        <source src="@/sounds/3.mp3" type="audio/mp3" />
      </audio>
      <audio id="noise4">
        <source src="@/sounds/4.mp3" type="audio/mp3" />
      </audio>
    </div>
  </div>
</template>

<script>

export default {
  data(){
    return{
      order: [],
      playerOrder: [],
      gameStart: false,
      round: 0,
      gameOver: false,
      intervalId: null,
      interval: 1500,
      on: false,
      flash: 0,
      one: false,
      second: false,
      third: false,
      fourty: false,
      easy: false,
      normal: false,
      hard: false

    }
  },
  methods:{
    startGame(){
      this.gameStart= true
      this.gameOver=false
      this.round = 1
      this.flash = 0
      for(let i=0; i<40; i++){
        this.order.push(Math.floor(Math.random()*4)+1)
      }
      this.intervalId = setInterval(this.gameTurn, Number(this.interval))
    },
    gameTurn(){
      this.on = false
      if(this.flash === this.round){
        clearInterval(this.intervalId)
        this.on = true
      }
      if(!this.on){
        setTimeout(()=>{
          if(this.order[this.flash]===1) {
            this.firstFlash()
          }
          if(this.order[this.flash]===2) {
            this.secondFlash()
          }
          if(this.order[this.flash]===3) {
            this.thirdFlash()
          }
          if(this.order[this.flash]===4) {
            this.fourFlash()
          }
          this.flash++

        }, 300)
      }
    },
    firstFlash(){
      this.one=true
      setTimeout(() => {
        let audio = document.getElementById('noise1')
        audio.play()
        this.one=false
      },400)
    },
    secondFlash(){
      this.second=true
      setTimeout(() => {
        let audio = document.getElementById('noise2')
        audio.play()
        this.second=false
      },400)
    },
    thirdFlash(){
      this.third=true
      setTimeout(() => {
        let audio = document.getElementById('noise3')
        audio.play()
        this.third=false
      },400)
    },
    fourFlash(){
      this.fourty=true
      setTimeout(() => {
        let audio = document.getElementById('noise4')
        audio.play()
        this.fourty=false
      },400)
    },
    firstClick(){
      if  (this.on){
        this.playerOrder.push(1)
        this.check()
        this.firstFlash()
      }
    },
    secondClick(){
      if  (this.on){
        this.playerOrder.push(2)
        this.check()
        this.secondFlash()
      }
    },
    thirdClick(){
      if  (this.on){
        this.playerOrder.push(3)
        this.check()
        this.thirdFlash()
      }
    },
    fourtyClick(){
      if  (this.on){
        this.playerOrder.push(4)
        this.check()
        this.fourFlash()
      }
    },
    check(){
      if(this.playerOrder[this.playerOrder.length-1] !== this.order[this.playerOrder.length-1]){
        this.gameOver=true
        this.gameStart=false
        this.on = false
        this.playerOrder = []
        this.order = []
      }
      if(this.round === this.playerOrder.length && !this.gameOver){
        this.round++
        this.playerOrder = []
        this.on = false
        this.flash=0
        this.intervalId = setInterval(this.gameTurn, Number(this.interval))
      }
    }
  }
}

</script>

<style>
  body{
    background-color: beige;
    margin: 0;
    padding: 0;
  }

  .container {
    width: 600px;
    margin: 0 auto;
  }

  h1{
    text-align: center;
  }

  .simon {
    background: #fff;
    position: relative;
    float: left;
    margin-right: 3em;
    width: 301px;
    height: 301px;
    -webkit-border-radius: 150px 150px 150px 150px;
    border-radius: 150px 150px 150px 150px;
    -moz-box-shadow: 2px 1px 12px #aaa;
    -webkit-box-shadow: 2px 1px 12px #aaa;
    -o-box-shadow: 2px 1px 12px #aaa;
    box-shadow: 2px 1px 12px #aaa;
  }

  ul, li {
    padding: 0;
    margin: 0;
  }

  ul {
    list-style: none;
    margin-left: 3px;
    margin-top: 3px;
  }


  .red, .blue, .yellow, .green {
    opacity: 0.6;
    height: 295px;
    -webkit-border-radius: 150px 150px 150px 150px;
    border-radius: 150px 150px 150px 150px;
    position: absolute;
    text-indent: 10000px;
  }

  .redOn, .blueOn, .yellowOn, .greenOn {
    opacity: 1;
    height: 295px;
    -webkit-border-radius: 150px 150px 150px 150px;
    border-radius: 150px 150px 150px 150px;
    position: absolute;
    text-indent: 10000px;
  }


  .red:hover, .blue:hover, .yellow:hover, .green:hover {
    border: 2px solid black;
  }

  .red {
    background: #FF5643;
    clip: rect(0px, 300px, 150px, 150px);
    width: 295px;
  }

  .blue {
    background: dodgerblue;
    clip: rect(0px, 150px, 150px, 0px);
    width: 295px;
  }

  .yellow {
    background: #FEEF33;
    clip: rect(150px, 150px, 300px, 0px);
    width: 295px;
  }

  .green {
    background: #BEDE15;
    clip: rect(150px,300px, 300px, 150px);
    width: 295px;
  }

  .game-info {
    margin-top: 90px;
  }

  .game-info button {
    width: 5em;
    box-sizing: border-box;
    font-size: 1.4em;
    -webkit-border-radius: 10px 10px 10px 10px;
    border-radius: 10px 10px 10px 10px;
    background: #6DABE8;
    border: none;
    padding: 0.3em 0.6em;
  }


  .game-info button:hover {
    background: #78BCFF;
  }

  p[data-action="lose"]{
    height: 10px;
  }

  .game-options h2 {
    margin-top: 10px;
    margin-bottom: 0;
  }

  .game-options input[type="radio"] {
    margin-right: 10px;
  }
</style>