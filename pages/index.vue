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
        <button @click="startGame">Play Again</button>
          <p 
          v-if="isWinner"
          class="fine"
          >You Won </p>
          <p
          v-else
          class="danger"
          > You Lost </p>
      </template>

    </div>

    <div 
    v-if="!log.length == 0"
    class="painel three">
      <ul>
        <ol 
        v-for="linha in log" 
        class="linha-lista"
        :class="{fine: (linha.infoFrom == 'player') , danger: (linha.infoFrom == 'monster')}"
        :key="linha" >{{linha.info}}</ol>
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
      this.isWinner = null
    },
    attack(){
      this.player.life = this.player.life - 10
      this.monster.life = this.monster.life - 8

      this.log.push({
        infoFrom: "player",
        info: "Player attacked - " + 8 + " pontos "
        })

      this.log.push({
        infoFrom: "monster",
        info: "Monster attacked - " + 10 + " pontos "
        })

      this.result
      this.BarColor
    },
    specialAttack(){
      this.player.life = this.player.life - 8
      this.monster.life = this.monster.life - 10

            this.log.push({
        infoFrom: "player",
        info: "Player attacked - " + 10 + " pontos "
        })

      this.log.push({
        infoFrom: "monster",
        info: "Monster attacked - " + 8 + " pontos "
        })

      this.result
      this.BarColor
    },
    cure(){
      if (this.player.life <= 90){
        this.player.life = this.player.life + 10
      }else{
        this.player.life = 100
      }

        this.log.push({
        infoFrom: "player",
        info: "Player cured - " + 10 + " pontos "
        })

      this.BarColor
    },
    quitGame(){
      this.isPlaying = !this.isPlaying
      this.player.life = 100
      this.monster.life = 100

      this.log = []
    }
  },
  computed:{
    result(){
      return this.player.life  <= 0 ?
             this.isWinner = false :
             this.monster.life <= 0 ?
             this.isWinner = true  : 
             null
    }
  }

}
</script>

<style>

body{ background-color: whitesmoke; }


.painel.one{
  display: flex;
  justify-content: space-around;
  text-align: center;
}

.painel.two{
  display: flex;
  justify-content: space-around;
  text-align: center;
}
.painel.two button{
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
  background-color: rgb(9, 202, 9);
}

.danger{
  background-color: rgb(194, 1, 1);
}


.painel{
  font-size: 30px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  margin: 50px auto;
  background-color: white;
  box-shadow: 0 0 0.8em rgb(182, 182, 182);
  width: 70vw;
  padding: 50px;
}

.linha-lista{
  margin: 10px;
  border-radius: 10px;
}


</style>