<template>
    <div class="card-component">
        <b-card
            :title="title"
            :img-src="image"
            :img-alt="title"
            :class="state"
        >
            <b-card-text>
                <span class="points d-flex justify-content-center align-items-center">{{points}}</span>
            </b-card-text>
        </b-card>
    </div>
</template>

<script>
export default {
  name: 'card-component',
  data () {
    return {}
  },
  props: {
    title: {
      type: String,
      required: true
    },
    image: {
      type: String,
      required: true
    },
    points: {
      type: Number,
      required: true
    },
    state: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped>
    .card {
      position: relative;
      border: 2px solid #A0522D;
      border-radius: 8px;
    }
    .card.opponents-hand {
      border: 2px solid #c0c0c0;
    }
    .players-hand {
      box-shadow: 10px 10px 32px black;
      transition: all ease 1s;
    }
    .players-hand:hover {
      transform: scale(1.1);
    }
    .points {
      position: absolute;
      top: 0;
      left: 0;
      font-size: 22px;
      font-weight: 700;
      color: #fff;
      background-image: url('../assets/points-canvas.svg');
      background-position: center;
      background-size: cover;
      border-radius: 100%;
      height: 50px;
      width: 50px;
    }
    .card-img {
      border-radius: 5px 5px 0 0;
    }
    .card-body {
      background-image: radial-gradient(circle, #FAFAD2, #A0522D);
      border-radius: 0 0 5px 5px;
    }
    .card-title {
      font-size: 12px;
      color: black;
      text-align: center;
      opacity: 0.8;
    }
    .opponents-hand .points, .opponents-hand .card-body {
      display: none;
    }
    .opponents-hand .card-img {
      border-radius: 5px;
      max-width: 100%;
      height: auto;
    }
</style>
