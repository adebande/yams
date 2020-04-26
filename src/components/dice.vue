<template>
    <div id="dice">
        <div id="diceImage">
            <img :src="diceImg">
        </div>
        <div id="rollButton">
            <b-button @click="roll">Roll dice</b-button>
        </div>
    </div>
</template>


<script>
    export default {
        name: 'dice',
        data() {
            return {
                diceValue: 6,
                diceImg: require('../assets/dice6.png')
            }
        },
        methods : {
            showDice() {
                if (this.diceValue === 1) {
                    this.diceImg = require("../assets/dice1.png")
                } else if (this.diceValue === 2) {
                    this.diceImg = require('../assets/dice2.png')
                } else if (this.diceValue === 3) {
                    this.diceImg = require('../assets/dice3.png') 
                } else if (this.diceValue === 4) {
                    this.diceImg = require('../assets/dice4.png') 
                } else if (this.diceValue === 5) {
                    this.diceImg = require('../assets/dice5.png') 
                } else if (this.diceValue === 6) {
                    this.diceImg = require('../assets/dice6.png')  
                }
            },
            roll() {
                this.diceValue = Math.floor(Math.random() * 6) + 1
                this.showDice()
            },
        },
    }
</script>

<style>

#diceImage {
    padding: 5px;
}

#rollButton {
    padding: 5px;
}

</style>