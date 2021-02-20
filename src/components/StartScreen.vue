<template>
    <div class="start-game text-center">
      <main role="main" class="inner cover">
        <h1 class="cover-heading">Memory Game</h1>
        <p class="lead">
          <button href="#" class="btn btn-lg btn-secondary"
            v-on:click="startGame"
          >Start Game</button>
        </p>
      </main>
    </div>
</template>

<script>
export default {
    name: 'StartScreen',
    methods: {
        startGame() {
            this.$emit('handleStartGame');
        }
    }
}
</script>

<style scoped>
    .start-game {
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
        width: 400px;
        height: 200px;
    }
    h1 {
        margin-bottom: 50px;
    }
</style>