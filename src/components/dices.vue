<template>
    <div id="dices">
        <div id="dicesImages">
            <img :src="dices[0].diceImg">
            <input type="checkbox" v-model="dices[0].keep">
            <img :src="dices[1].diceImg">
            <input type="checkbox" v-model="dices[1].keep">
            <img :src="dices[2].diceImg">
            <input type="checkbox" v-model="dices[2].keep">
            <img :src="dices[3].diceImg">
            <input type="checkbox" v-model="dices[3].keep">
            <img :src="dices[4].diceImg">
            <input type="checkbox" v-model="dices[4].keep">
        </div>
        <div id="rollButton">
            <b-button @click="roll">Roll</b-button>
        </div>
    </div>
</template>


<script>
    export default {
        name: 'dices',
        data() {
            return {
                dices: [
                    {id: 0, diceValue: 6, diceImg: require('../assets/dice6.png'), keep: false},
                    {id: 1, diceValue: 6, diceImg: require('../assets/dice6.png'), keep: false},
                    {id: 2, diceValue: 6, diceImg: require('../assets/dice6.png'), keep: false},
                    {id: 3, diceValue: 6, diceImg: require('../assets/dice6.png'), keep: false},
                    {id: 4, diceValue: 6, diceImg: require('../assets/dice6.png'), keep: false}
                ]
            }
        },
        methods : {
            showDice(elt) {
                if (elt.diceValue === 1) {
                    elt.diceImg = require("../assets/dice1.png")
                } else if (elt.diceValue === 2) {
                    elt.diceImg = require('../assets/dice2.png')
                } else if (elt.diceValue === 3) {
                    elt.diceImg = require('../assets/dice3.png') 
                } else if (elt.diceValue === 4) {
                    elt.diceImg = require('../assets/dice4.png') 
                } else if (elt.diceValue === 5) {
                    elt.diceImg = require('../assets/dice5.png') 
                } else if (elt.diceValue === 6) {
                    elt.diceImg = require('../assets/dice6.png')  
                }
            },
            roll() {
                for (let dice of this.dices) {
                    if (dice.keep === false) {
                        dice.diceValue = Math.floor(Math.random() * 6) + 1
                        this.showDice(dice)
                    }
                }
            },
        },
    }
</script>

<style>

#dicesImages {
    padding: 5px;
}

#rollButton {
    padding: 5px;
}

</style>