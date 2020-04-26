<template>
  <div id="app">
    <div id="header">
      <b-jumbotron fluid header="Yam's" lead="2 players game"></b-jumbotron>
    </div>
    <div id="dices">
      <dices></dices>
    </div>
  </div>
</template>

<script>

import dices from './components/dices'

export default {
  name: 'App',
  components: {
    dices
  }
}
</script>

<style>

#app {

}

#dices {
  text-align: center;
}

</style>
