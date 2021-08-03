/* eslint-disable vue/valid-template-root */
<template>
  <div id="app">
    <b-container>
      <div class="actions">
        <b-button @click="draw()" :disabled="disableDraw">Sortear carta</b-button>
        <b-button @click="endRound()" :disabled="disableEnd">Encerrar rodada</b-button>

        <p>Adversário: {{this.opponentPoints}}</p>
        <p>Jogador: {{this.playerPoints}}</p>

        <p>Vidas do Adversário: {{this.opponentsLife}}</p>
        <p>Vidas do Jogador: {{this.playersLife}}</p>
      </div>

      <!-- Mão do Adversário -->
      <b-row align-h="center" class="opponent-cards">
        <template v-if="this.opponentCards.length > 0">
          <b-col md="2" v-for="opponentCard in opponentCards" :key="opponentCard.id">
              <card
                :title="opponentCard.title"
                :image="require('@/assets/card_back.png')"
                :points="opponentCard.points"
                state="opponents-hand"
              ></card>
          </b-col>
        </template>
        <template v-else>
          <b-col md="12">Mão vazia</b-col>
        </template>
      </b-row>

      <!-- Mesa -->
      <b-row align-h="center" class="opponent-table">
        <template v-if="this.opponentTable.length > 0">
          <b-col md="2" v-for="opponentTableCard in opponentTable" :key="opponentTableCard.id">
            <card
              :title="opponentTableCard.title"
              :image="opponentTableCard.image"
              :points="opponentTableCard.points"
              state="opponents-table"
            ></card>
          </b-col>
        </template>
        <template v-else>
          <b-col md="12">
            Mesa vazia
          </b-col>
        </template>
      </b-row>
      <b-row align-h="center" class="my-table">
        <template v-if="this.myTable.length > 0">
          <b-col md="2" v-for="myTableCard in myTable" :key="myTableCard.id">
            <card
              :title="myTableCard.title"
              :image="myTableCard.image"
              :points="myTableCard.points"
              state="players-table"
            ></card>
          </b-col>
        </template>
        <template v-else>
          <b-col md="12">Mesa vazia</b-col>
        </template>
      </b-row>

      <!-- Mão do Jogador -->
      <b-row align-h="center" class="my-cards">
        <template v-if="this.playerCards.length > 0">
          <b-col md="2" v-for="playerCard in playerCards" :key="playerCard.id">
            <a href="#" class="card-action" @click="selectCard(playerCard)">
              <card
                :title="playerCard.title"
                :image="playerCard.image"
                :points="playerCard.points"
                state="players-hand"
              ></card>
            </a>
          </b-col>
        </template>
        <template v-else>
          <b-col md="12">Mão vazia</b-col>
        </template>
      </b-row>
    </b-container>

    <b-modal ref="bv-modal-example" :title="result.title" centered hide-footer hide-header>
      <div class="d-block text-center">
        <h3>{{result.message}}</h3>
      </div>
    </b-modal>

  </div>
</template>

<script>
import Card from './components/Card.vue'

export default {
  name: 'App',
  components: {
    Card
  },
  data () {
    return {
      opponentCards: [],
      playerCards: [],

      myTable: [],
      opponentTable: [],

      disableDraw: false,
      disableEnd: true,

      playerPoints: 0,
      opponentPoints: 0,

      initialHandCards: 4,

      playersOptEnd: false,
      opponentsOptEnd: false,

      opponentsLife: 2,
      playersLife: 2,

      rangeToEndRound: 22,

      result: {
        title: null,
        message: null
      },

      cards: [
        {
          id: 0,
          title: 'Geralt de Rívia',
          image: require('@/assets/card_geralt.jpg'),
          points: 15
        },
        {
          id: 1,
          title: 'Cirilla Fiona Elen Riannon',
          image: require('@/assets/card_ciri.png'),
          points: 15
        },
        {
          id: 2,
          title: 'Yennefer de Vengerberg',
          image: require('@/assets/card_yennefer.jpg'),
          points: 7
        },
        {
          id: 3,
          title: 'Triss Merigold',
          image: require('@/assets/card_triss.jpg'),
          points: 7
        },
        {
          id: 4,
          title: 'Yennefer de Vengerberg',
          image: require('@/assets/card_yennefer.jpg'),
          points: 7
        },
        {
          id: 5,
          title: 'Triss Merigold',
          image: require('@/assets/card_triss.jpg'),
          points: 7
        },
        {
          id: 6,
          title: 'Triss Merigold',
          image: require('@/assets/card_triss.jpg'),
          points: 7
        },
        {
          id: 7,
          title: 'Yennefer de Vengerberg',
          image: require('@/assets/card_yennefer.jpg'),
          points: 7
        },
        {
          id: 8,
          title: 'Triss Merigold',
          image: require('@/assets/card_triss.jpg'),
          points: 7
        }
      ]
    }
  },

  methods: {
    draw () {
      let i

      for (i = 0; i < this.initialHandCards; i++) {
        var opponentCardNumber = parseInt(Math.random() * (this.cards.length - 0) + 0)
        this.opponentCards.push(this.cards[opponentCardNumber])
        this.cards.splice(opponentCardNumber, 1)

        var playerCardNumber = parseInt(Math.random() * (this.cards.length - 0) + 0)
        this.playerCards.push(this.cards[playerCardNumber])
        this.cards.splice(playerCardNumber, 1)
      }

      this.disableDraw = true
      this.disableEnd = false
    },

    selectCard (card) {
      this.myTable.push(card)
      this.playerPoints += card.points
      this.playerCards.splice(card, 1)

      var difference = this.opponentPoints - this.playerPoints

      if (this.opponentCards.length > 0) {
        if (difference < this.rangeToEndRound && this.opponentsOptEnd === false) {
          this.opponentSelectionCard()
        } else {
          this.opponentsOptEnd = true
        }
      } else {
        this.opponentsOptEnd = true
      }
    },

    opponentSelectionCard () {
      var opponentSelection = parseInt(Math.random() * (this.opponentCards.length - 0) + 0)

      this.opponentTable.push(this.opponentCards[opponentSelection])
      this.opponentPoints += this.opponentCards[opponentSelection].points
      this.opponentCards.splice(this.opponentCards[opponentSelection], 1)
    },

    endRound () {
      this.playersOptEnd = true

      while (this.playerPoints >= this.opponentPoints && this.opponentCards.length > 0) {
        this.opponentSelectionCard()
      }

      this.verifyWinner()
    },

    verifyWinner () {
      if (this.playerPoints > this.opponentPoints) {
        this.opponentsLife--
        alert('Você venceu essa rodada')
      } else if (this.opponentPoints > this.playerPoints) {
        this.playersLife--
        alert('Você perdeu essa rodada')
      } else {
        this.result.title = 'Empate'
        alert('Rodada empatada')
      }

      if (this.opponentsLife === 0 || this.playersLife === 0) {
        if (this.opponentsLife === 0) {
          this.result.title = 'Vitória'
          this.result.message = 'Você venceu'
        } else {
          this.result.title = 'Derrota'
          this.result.message = 'Você perdeu'
        }

        this.$refs['bv-modal-example'].show()
      } else {
        this.clearTable()
      }
    },

    clearTable () {
      this.opponentTable = []
      this.myTable = []

      this.opponentPoints = 0
      this.playerPoints = 0
    }
  }
}
</script>

<style scoped>
  .actions {
    position: fixed;
    top: 15px;
    left: 15px;
  }
 .card-action, .card-action:hover {
   text-decoration: none;
   color: #333;
 }
 button.close {
   border: none;
   background: transparent;
 }
</style>
