<template>
  <div id="app">
    <div id="header">
      <b-jumbotron fluid header="Yam's" lead="2 players game"></b-jumbotron>
    </div>
    <div id="dice">
      <dice></dice>
    </div>
  </div>
</template>

<script>

import dice from './components/dice'

export default {
  name: 'App',
  components: {
    dice
  }
}
</script>

<style>

#app {

}

#dice {
  text-align: center;
}

</style>
