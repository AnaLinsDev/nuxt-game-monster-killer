<template>
  <div class="monsters-page">

    <div class="painel one">

      <div class="player">
        Player
        <div class="life-bar">
          <div 
          class="life" 
          :class="{ fine: (player.life > 20) , danger: (player.life <= 20)}" 
          :style="{ width: player.life + '%' }" />
        </div>
        <p>{{player.life}}%</p>
      </div>

      <div class="monster">
        Monster
        <div class="life-bar">
          <div 
          class="life" 
          :class="{fine: (monster.life > 20) , danger: (monster.life <= 20)}"
          :style="{ width: monster.life + '%' }" /> 
        </div>
        <p>{{monster.life}}%</p>
      </div>

    </div>

    <div class="painel two ">

      <template v-if="!isPlaying && isWinner == null">
        <button @click="startGame">Play</button>
      </template>

      <template v-else-if="isPlaying && isWinner == null">
        <button @click="attack">Attack</button>
        <button @click="specialAttack">Special Attack</button>
        <button @click="cure">Cure</button>
        <button @click="quitGame">Quit</button>
      </template>

      <template v-if="isWinner != null">
        <button 
        class="final-button"
        @click="startGame">Play Again</button>
          <p 
          v-if="isWinner"
          class="fine final-message" > You Won </p>
          <p
          v-else
          class="danger final-message" > You Lost </p>
      </template>

    </div>

    <div 
    v-if="!log.length == 0"
    class="painel three">
      <ul>
        <ol 
        v-for="(linha, index) in log" 
        :key="index"
        class="line-list"
        :class="{fine: (linha.infoFrom == 'player') , danger: (linha.infoFrom == 'monster')}"
         >{{linha.info}}</ol>
      </ul>
    </div>

  </div>
</template>

<script>
export default {
  data(){
    return{
      isPlaying: false,
      isWinner:null,

      player: { life : 100 },
      monster:{ life: 100 },

      log : []
    }
  },
  methods:{
    startGame(){
      this.isPlaying = !this.isPlaying
      this.resetGame()
    },
    resetGame(){
      this.player.life = 100
      this.monster.life = 100
      this.log = []
      this.isWinner = null
    },
    attack(){
      this.player.life = Math.max(0, this.player.life - 10)
      this.monster.life = Math.max(0, this.monster.life - 8)
      this.log.unshift({
        infoFrom: "player",
        info: "Player attacked - " + 8 + " points "
        })

      this.log.unshift({
        infoFrom: "monster",
        info: "Monster attacked - " + 10 + " points "
        })

      this.result

    },
    specialAttack(){
      this.player.life = Math.max(0, this.player.life - 8)
      this.monster.life = Math.max(0, this.monster.life - 10)

      this.log.unshift({
        infoFrom: "player",
        info: "Player attacked - " + 10 + " points "
        })

      this.log.unshift({
        infoFrom: "monster",
        info: "Monster attacked - " + 8 + " points "
        })

      this.result
  
    },
    cure(){
        if (this.player.life < 100){
          this.player.life = Math.min(100, this.player.life + 10) 

          this.log.unshift({
          infoFrom: "player",
          info: "Player cured - " + 10 + " points "
          })
        }
        
    }

    ,
    quitGame(){
      this.isPlaying = !this.isPlaying
      this.resetGame()
    }
  },
  computed:{
    result(){
      return this.player.life  <= 0 ?
             this.isWinner = false :
             this.monster.life <= 0 ?
             this.isWinner = true  : null
    }
  },
  watch:{
    result(n,o){
      console.log('novo: ', n);
      console.log('antigo: ', o);
    }
  }

}
</script>

<style >
@import url('https://fonts.googleapis.com/css2?family=Comfortaa&display=swap');

body{ 
  background-color: whitesmoke;
  font-family: 'Comfortaa', cursive;
}


.one{
  display: flex;
  justify-content: space-around;
  text-align: center;
}

.two{
  display: flex;
  justify-content: space-around;
  text-align: center;
}
.two button{
  font-size: 20px;
  padding: 10px 30px;
}

.life-bar{
  height: 30px;
  width: 400px;
  border: 2px solid black;
}

.life-bar .life{
  height: 100%;
}

.fine{
  background-color: rgb(7, 231, 7);
}

.danger{
  background-color: rgb(255, 56, 56);
}

.painel{
  font-size: 30px;
  margin: 50px auto;
  background-color: white;
  box-shadow: 0 0 0.8em rgb(182, 182, 182);
  width: 70vw;
  padding: 50px;
}

.line-list{
  margin: 10px;
  border-radius: 10px;
  text-align: center;
  padding: 5px;
}

.final-message{
  padding: 15px;
  border-radius: 10px;
}

.final-button{
  margin: auto 0;
  height: 70px;
  padding: 25px;
}

@media (max-width: 600px){
.one, .two{
  flex-direction: column;
}

.one{
  margin: 30px auto;
}
.life-bar{
  width: 100%;
}
.painel{
  font-size: 20px;
  padding: 30px;
}
.three ul{
  margin: 30px auto;
  padding:0;
}
}



</style>