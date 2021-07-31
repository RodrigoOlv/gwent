<template>
    <div class="result">
          <b-modal ref="result" :title="title">
            <p class="my-4">Hello from modal!</p>
        </b-modal>
    </div>
</template>

<script>
export default {
  name: 'result',
  props: {
    title: {
      type: String,
      required: true
    }
  }
}
</script>
