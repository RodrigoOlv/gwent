<template>
  <div id="app">
    <b-container>
      <div class="actions">
        <b-button @click="draw()" :disabled="disableDraw">Sortear cartas</b-button>
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
                :image="require('@/assets/card_back.svg')"
                :points="opponentCard.points"
                state="opponents-hand"
              ></card>
          </b-col>
        </template>
        <template v-else>
          <b-col md="12" class="no-cards"></b-col>
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
          <b-col md="12" class="no-cards"></b-col>
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
          <b-col md="12" class="no-cards"></b-col>
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
          <b-col md="12" class="no-cards"></b-col>
        </template>
      </b-row>
    </b-container>

    <b-modal ref="bv-modal-example" :title="result.title" centered hide-header hide-footer>
      <div class="d-block text-center">
        <img :src="result.icon" alt="" class="img-fluid">
        <h3 class="text-center">{{result.message}}</h3>
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

      initialHandCards: 6,

      playersOptEnd: false,
      opponentsOptEnd: false,

      opponentsLife: 2,
      playersLife: 2,

      rangeToEndRound: 5,

      result: {
        title: null,
        message: null,
        icon: null
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
          image: require('@/assets/card_ciri.jpg'),
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
          title: 'Eredin Bréacc Glas',
          image: require('@/assets/card_eredin.jpg'),
          points: 13
        },
        {
          id: 6,
          title: 'Avallac\'n',
          image: require('@/assets/card_avallach.jpg'),
          points: 7
        },
        {
          id: 7,
          title: 'Dandelion',
          image: require('@/assets/card_dandelion.jpg'),
          points: 2
        },
        {
          id: 8,
          title: 'Zoltan Chivay',
          image: require('@/assets/card_zoltan.jpg'),
          points: 5
        },
        {
          id: 8,
          title: 'Sigsmund Dijsktra',
          image: require('@/assets/card_dijkstra.jpg'),
          points: 6
        },
        {
          id: 9,
          title: 'Foltest de Teméria',
          image: require('@/assets/card_foltest.jpg'),
          points: 8
        },
        {
          id: 10,
          title: 'Emyr var Emreis',
          image: require('@/assets/card_emyr.jpg'),
          points: 10
        },
        {
          id: 11,
          title: 'Radovid V',
          image: require('@/assets/card_radovid.jpg'),
          points: 10
        },
        {
          id: 12,
          title: 'Vesemir',
          image: require('@/assets/card_vesemir.jpg'),
          points: 6
        },
        {
          id: 13,
          title: 'Lambert',
          image: require('@/assets/card_lambert.jpg'),
          points: 6
        },
        {
          id: 14,
          title: 'Eskel',
          image: require('@/assets/card_eskel.jpg'),
          points: 6
        },
        {
          id: 15,
          title: 'Phillipa Eilhart',
          image: require('@/assets/card_phillipa.jpg'),
          points: 6
        },
        {
          id: 16,
          title: 'Keira Metz',
          image: require('@/assets/card_keira.jpg'),
          points: 5
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

      window.scrollTo(0, document.body.scrollHeight)

      this.toogleMusic()
    },

    selectCard (card) {
      this.myTable.push(card)
      this.playerPoints += card.points

      for (let i = 0; i < this.playerCards.length; i++) {
        if (card.id === this.playerCards[i].id) {
          this.playerCards.splice(i, 1)
        }
      }

      var differenceToPlayer = this.opponentPoints - this.playerPoints
      var differenceToOpponent = this.playerPoints - this.opponentPoints

      if (this.opponentCards.length > 0) {
        if (differenceToOpponent < this.rangeToEndRound) {
          this.opponentsOptEnd = true
        } else {
          if (differenceToPlayer < this.rangeToEndRound && this.opponentsOptEnd === false) {
            setTimeout(this.opponentSelectionCard(), 3000)
            // setTimeout(() => this.opponentSelectionCard(), 1000)
          } else {
            this.opponentsOptEnd = true
          }
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

      var differenceToOpponent = this.playerPoints - this.opponentPoints

      while (differenceToOpponent <= this.rangeToEndRound && this.opponentCards.length > 0) {
        setTimeout(this.opponentSelectionCard(), 3000)
      }

      setTimeout(this.verifyWinner(), 3000)
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
          this.result.icon = require('@/assets/the-witcher.png')
        } else {
          this.result.title = 'Derrota'
          this.result.message = 'Você perdeu'
          this.result.icon = require('@/assets/wild-hunt.png')
        }

        this.$refs['bv-modal-example'].show()
        this.disableEnd = true
        this.clearPlayersHand()
        this.clearOpponentsHand()
      } else if (this.opponentCards.length === 0 && this.playerCards.length === 0) {
        this.result.title = 'Derrota'
        this.result.message = 'Você perdeu'
        this.$refs['bv-modal-example'].show()
        this.disableEnd = true
      } else {
        this.clearTable()
      }
    },

    clearTable () {
      this.opponentTable = []
      this.myTable = []

      this.opponentPoints = 0
      this.playerPoints = 0
    },

    clearPlayersHand () {
      this.playerCards = []
    },

    clearOpponentsHand () {
      this.opponentCards = []
    },

    toogleMusic () {
      var audioPath = require('@/assets/song.mp3')

      var audio = new Audio(audioPath)

      if (typeof audio.loop === 'boolean') {
        audio.loop = true
      } else {
        audio.addEventListener('ended', function () {
          this.currentTime = 0
          this.play()
        }, false)
      }

      audio.play()
    }
  }
}
</script>

<style scoped>
  #app {
    color: white;
  }
  .actions {
    position: fixed;
    top: 15px;
    left: 15px;
    z-index: 2;
  }
  .card-action, .card-action:hover {
    text-decoration: none;
    color: #333;
  }
  button.close {
    border: none;
    background: transparent;
  }
  .no-cards {
    min-height: 400px;
  }
  .my-cards, .opponent-cards, .opponent-table, .my-table {
    padding: 20px;
  }
  .modal-body {
    background: #fff;
  }
  button.close {
    background: none !important;
    border: none !important;
    font-size: 25px !important;
  }
</style>
