<template>
	<div>
		<template v-if="!isStart">
      <StartScreen
        v-on:handleStartGame="handleStartGame"
      />
		</template>
    <template v-else>
      <template v-if="theme === null">
        <SelectTheme
          v-on:handleSelectTheme="handleSelectTheme"
        />
      </template>
      <template v-else>
        <Game
          v-bind:cardData="cardData"
          v-on:handleResetGame="handleResetGame"
        />
      </template>
    </template>
	</div>
</template>
<script>
import Game from './components/Game.vue'
import SelectTheme from './components/SelectTheme.vue'
import StartScreen from './components/StartScreen.vue'
import data from './data.json'

export default {
  name: 'App',
  data() {
    return {
      isStart: false,
      theme: null,
      cardData: []
    }
  },
  methods: {
    handleStartGame() {
      this.isStart = true;
      console.log(data);
    },
    handleSelectTheme(theme) {
      this.theme = theme;

      if(theme === 'fruit') {
        this.cardData = data.fruitData;
      }
      else {
        this.cardData = data.girlData;
      }
    },
    handleResetGame() {
      this.isStart = false;
      this.theme = null;
    }
  },
  components: {
    Game,
    StartScreen,
    SelectTheme
  },

}
</script>


