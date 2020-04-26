<template>
    <div id="dices">
        <p>Player {{player}}</p>
        <p>{{tries}} {{triesText}} left.</p>
        <div id="dicesImages">
            <img :src="dices[0].diceImg" :class="dices[0].activeDice" @click="keepDice(dices[0])">
            <img :src="dices[1].diceImg" :class="dices[1].activeDice" @click="keepDice(dices[1])">
            <img :src="dices[2].diceImg" :class="dices[2].activeDice" @click="keepDice(dices[2])">
            <img :src="dices[3].diceImg" :class="dices[3].activeDice" @click="keepDice(dices[3])">
            <img :src="dices[4].diceImg" :class="dices[4].activeDice" @click="keepDice(dices[4])">
        </div>
        <div id="rollButton">
            <b-button class="buttons" @click="roll" :disabled="disableRoll">Roll</b-button>
            <b-button class="buttons" @click="nextPlayer" :disabled="disableNextPlayer">Next Player</b-button>
        </div>
    </div>
</template>


<script>
    export default {
        name: 'dices',
        data() {
            return {
                dices: [
                    {id: 0, diceValue: 0, diceImg: require('../assets/diceempty.png'), keep: false, activeDice: "inactiveDice"},
                    {id: 1, diceValue: 0, diceImg: require('../assets/diceempty.png'), keep: false, activeDice: "inactiveDice"},
                    {id: 2, diceValue: 0, diceImg: require('../assets/diceempty.png'), keep: false, activeDice: "inactiveDice"},
                    {id: 3, diceValue: 0, diceImg: require('../assets/diceempty.png'), keep: false, activeDice: "inactiveDice"},
                    {id: 4, diceValue: 0, diceImg: require('../assets/diceempty.png'), keep: false, activeDice: "inactiveDice"},
                ],
                player: 1,
                tries: 3,
                triesText : 'tries',
                disableRoll : false,
                disableNextPlayer : true,
            }
        },
        methods : {
            showDice(elt) {
                if (elt.diceValue === 0) {
                    elt.diceImg = require("../assets/diceempty.png")
                } else if (elt.diceValue === 1) {
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
            keepDice(elt) {
                if (elt.keep === false && elt.diceValue !==0){
                    elt.keep = true
                    elt.activeDice = "activeDice"
                } else if (elt.keep === true || elt.diceValue === 0) {
                    elt.keep = false
                    elt.activeDice = "inactiveDice"
                }
            },
            nextPlayer() {
                this.tries = 3
                if (this.player === 1) {
                    this.player = 2
                } else if (this.player === 2) {
                    this.player = 1
                }
                this.disableRoll = false
                for (let dice of this.dices) {
                    dice.diceValue = 0
                    dice.keep = false
                    dice.activeDice = "inactiveDice"
                    this.showDice(dice)
                }
                this.disableNextPlayer = true
            },
            roll() {
                for (let dice of this.dices) {
                    if (dice.keep === false) {
                        dice.diceValue = Math.floor(Math.random() * 6) + 1
                        this.showDice(dice)
                    }
                }
                this.tries -= 1
                if (this.tries <= 1) {
                    this.triesText = "try"
                }
                if (this.tries === 0) {
                    this.disableRoll = true
                }
                this.disableNextPlayer = false 
            },
        },
    }
</script>

<style>

#dicesImages {

}

#dicesImages .inactiveDice {
    margin: 12px;
}

#dicesImages .activeDice {
    margin: 7px;
    border: 5px solid green;
}

#rollButton {
    padding: 5px;
}

#rollButton .buttons {
    margin: 5px;
}

</style>