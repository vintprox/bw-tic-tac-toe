<template>
  <div class="container">
    <TTTField
      @win="announceWinner"
    />
    <div v-if="winner">
      Побеждает игрок №{{ winner }}!
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    winner: null
  }),
  methods: {
    announceWinner (playerNumber) {
      this.winner = playerNumber
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  font-family:
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
}
</style>
