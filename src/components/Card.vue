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
    }
    .players-hand {
        transition: all ease 1s;
    }
    .players-hand:hover {
        transform: scale(1.1);
    }
    .players-hand .card-img {
        max-height: 350px;
        width: auto;
    }
    .points {
        position: absolute;
        top: 0;
        left: 0;
        font-size: 22px;
        font-weight: 700;
        color: #fff;
        background: #000;
        border: 3px solid coral;
        border-radius: 100%;
        height: 50px;
        width: 50px;
    }
    .card-title {
        font-size: 16px;
        text-align: center;
    }
    .opponents-hand .points, .opponents-hand .card-body {
        display: none;
    }
</style>
