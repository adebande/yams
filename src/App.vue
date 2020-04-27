<template>
  <div id="app">
    <div id="header">
      <b-jumbotron fluid header="Yam's" lead="2 players game" id="jumbo">
        <b-button variant="danger" @click="newGame" class="buttons">New Game</b-button>
        <b-button variant="info" class="buttons" @click="$bvModal.show('howtoplay-modal')">How to play</b-button>
      </b-jumbotron>
      <b-modal id="howtoplay-modal" hide-footer>
        <template v-slot:modal-title>
          How to play
        </template>
        <div class="d-block text-center">
          <h3>Welcome to my Yam's App !</h3>
          <p>To start playing, press "Roll" to roll all the dices.</p>
          <p>You've got 3 attempts to get one of the dices combinations.</p>
          <p>For each attempt, you can choose to re-roll as many dices as you want. Simply click on the dices you want to re-roll and press "Roll".</p>
          <p>You can also choose which cell of the grid you want to fill. Choose among the items on the scrolling menu and press "Submit".<p/>
          <p>Then press "Next Player" to skip to player 2.</p>
          <p>For detailed Yam's / Yahtzee rules you may visit this <a href="https://en.wikipedia.org/wiki/Yahtzee" target="blank">Wikipedia page</a>.</p>


          
        </div>
        <b-button class="mt-3" variant="success" block @click="$bvModal.hide('howtoplay-modal')">Play Now !</b-button>
      </b-modal>
    </div>
    <div id="dices">
      <dices v-if="renderComponent"></dices>
    </div>
  </div>
</template>

<script>

import dices from './components/dices'

export default {
  name: 'App',
  components: {
    dices
  },
  data: function() {
    return {
      renderComponent: true
    }
  },
  methods: {
    newGame() {
      this.renderComponent = false;  
      this.$nextTick(() => {
          this.renderComponent = true;
        });
      }
    }
}
</script>

<style>

#app {
  text-align: center;
  background-color: var(--light);
}

#header .buttons {
  margin: 5px;
  box-shadow: 5px 5px 5px var(--secondary);
}

#dices {
  text-align: center;
}

</style>
