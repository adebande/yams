<template>
    <div id="dices">
        <b-container>
            <p><strong>Player {{player}}</strong></p>
            <p>{{tries}} {{triesText}} left.</p>
            <div id="dicesImages">
                <img :src="dices[0].diceImg" :class="dices[0].activeDice" @click="rerollDice(dices[0])">
                <img :src="dices[1].diceImg" :class="dices[1].activeDice" @click="rerollDice(dices[1])">
                <img :src="dices[2].diceImg" :class="dices[2].activeDice" @click="rerollDice(dices[2])">
                <img :src="dices[3].diceImg" :class="dices[3].activeDice" @click="rerollDice(dices[3])">
                <img :src="dices[4].diceImg" :class="dices[4].activeDice" @click="rerollDice(dices[4])">
            </div>
            <div id="selector"> 
                <b-select v-model="selected" :options="combinations" @change="showCount"></b-select>
                <div class="mt-3">Selected: <strong>{{ selected }} {{ptsCount}}</strong></div>
            </div>
            <div id="rollButton">
                <b-button class="buttons" @click="roll" :disabled="disableRoll">Roll</b-button>
                <b-button class="buttons" @click="nextPlayer" :disabled="disableNextPlayer">Next Player</b-button>
            </div>
            <div id="scoreSheet">
                
            </div>
        </b-container>
    </div>
</template>


<script>
    export default {
        name: 'dices',
        data() {
            return {
                dices: [
                    {id: 0, diceValue: 0, diceImg: require('../assets/diceempty.png'), reroll: true, activeDice: "activeDice"},
                    {id: 1, diceValue: 0, diceImg: require('../assets/diceempty.png'), reroll: true, activeDice: "activeDice"},
                    {id: 2, diceValue: 0, diceImg: require('../assets/diceempty.png'), reroll: true, activeDice: "activeDice"},
                    {id: 3, diceValue: 0, diceImg: require('../assets/diceempty.png'), reroll: true, activeDice: "activeDice"},
                    {id: 4, diceValue: 0, diceImg: require('../assets/diceempty.png'), reroll: true, activeDice: "activeDice"},
                ],
                player: 1,
                tries: 3,
                triesText : 'tries',
                disableRoll : false,
                disableNextPlayer : true,
                count : 0,
                ptsCount: null,
                brelan : "false",
                selected: null,
                combinations: [
                    { value: null, text: 'Select a combination'},
                    { value: 'One', text: 'One', points: 0 },
                    { value: 'Two', text: 'Two', points: 0 },
                    { value: 'Three', text: 'Three', points: 0 },
                    { value: 'Four', text: 'Four', points: 0 },
                    { value: 'Five', text: 'Five', points: 0 },
                    { value: 'Six', text: 'Six', points: 0 },
                    { value: 'Plus', text: 'Plus', points: 0 },
                    { value: 'Minus', text: 'Minus', points: 0},
                    { value: 'Straight', text: 'Straight', points: 0 },
                    { value: 'Full', text: 'Full', points: 0 },
                    { value: '4 of a kind', text: '4 of a kind', points: 0 },
                    { value: "Yam's", text: "Yam's", points: 0 },
                ],
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
            rerollDice(elt) {
                if (elt.reroll === true && elt.diceValue !==0){
                    elt.reroll = false
                    elt.activeDice = "inactiveDice"
                } else if (elt.reroll === false && this.tries > 0) {
                    elt.reroll = true
                    elt.activeDice = "activeDice"
                }
            },
            nextPlayer() {
                this.tries = 3
                this.triesText = "tries"
                this.ptsCount = null
                this.selected = null
                for ( let combination of this.combinations) {
                    combination.points = 0
                }
                if (this.player === 1) {
                    this.player = 2
                } else if (this.player === 2) {
                    this.player = 1
                }
                this.disableRoll = false
                for (let dice of this.dices) {
                    dice.diceValue = 0
                    dice.reroll = true
                    dice.activeDice = "activeDice"
                    this.showDice(dice)
                }
                this.disableNextPlayer = true
            },
            counter(elt, d1, d2, d3, d4, d5) {
                this.count = 0
                if (elt == d1) {this.count +=1}
                if (elt == d2) {this.count +=1}
                if (elt == d3) {this.count +=1}
                if (elt == d4) {this.count +=1}
                if (elt == d5) {this.count +=1}
                return this.count
            },
            calculChiffres() {
                this.combinations[1].points = this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue)
                this.combinations[2].points = this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) * 2
                this.combinations[3].points = this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) * 3
                this.combinations[4].points = this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) * 4
                this.combinations[5].points = this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) * 5
                this.combinations[6].points = this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) * 6
            },
            calculPlusMoins() {
                this.combinations[7].points = this.dices[0].diceValue + this.dices[1].diceValue + this.dices[2].diceValue + this.dices[3].diceValue + this.dices[4].diceValue
                this.combinations[8].points = this.dices[0].diceValue + this.dices[1].diceValue + this.dices[2].diceValue + this.dices[3].diceValue + this.dices[4].diceValue
            },
            calculSuite() {
                if ( this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 && this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 && this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 && this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 && this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 ) {
                    this.combinations[9].points = 20
                } else if ( this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 && this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 && this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 && this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 && this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 ) {
                    this.combinations[9].points = 20
                }
            },
            calculFull() {
                if ( this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 3 && (this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 )) {
                    this.combinations[10].points = 30
                } else if ( this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 3 && (this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 )) {
                    this.combinations[10].points = 30
                } else if ( this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 3 && (this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 )) {
                    this.combinations[10].points = 30
                } else if ( this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 3 && (this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 )) {
                    this.combinations[10].points = 30
                } else if ( this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 3 && (this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 )) {
                    this.combinations[10].points = 30
                } else if ( this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 3 && (this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 )) {
                    this.combinations[10].points = 30
                }
            },
            calculCarre() {
                if ( this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 4 || this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 4 || this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 4 || this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 4 || this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 4 || this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 4 ) {
                    this.combinations[10].points = 30
                    this.combinations[11].points = 40
                }
            },
            calculYams() {
                if ( this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 5 || this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 5 || this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 5 || this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 5 || this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 5 || this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 5 ) {
                    this.combinations[10].points = 30
                    this.combinations[11].points = 40
                }
            },
            showCount() {
                switch (this.selected) {
                    case "One":
                        this.ptsCount = "( " + this.combinations[1].points + " points)"
                        break;
                    case "Two":
                        this.ptsCount = "( " + this.combinations[2].points + " points)"
                        break;
                    case "Three":
                        this.ptsCount = "( " + this.combinations[3].points + " points)"
                        break;
                    case "Four":
                        this.ptsCount = "( " + this.combinations[4].points + " points)"
                        break;
                    case "Five":
                        this.ptsCount = "( " + this.combinations[5].points + " points)"
                        break;
                    case "Six":
                        this.ptsCount = "( " + this.combinations[6].points + " points)"
                        break;
                    case "Plus":
                        this.ptsCount = "( " + this.combinations[7].points + " points)"
                        break;
                    case "Minus":
                        this.ptsCount = "( " + this.combinations[8].points + " points)"
                        break;
                    case "Straight":
                        this.ptsCount = "( " + this.combinations[9].points + " points)"
                        break;
                    case "Full":
                        this.ptsCount = "( " + this.combinations[10].points + " points)"
                        break;
                    case "4 of a kind":
                        this.ptsCount = "( " + this.combinations[11].points + " points)"
                        break;
                    case "Yam's":
                        this.ptsCount = "( " + this.combinations[12].points + " points)"
                        break;
                    default:
                        this.ptsCount = null
                }
            },
            roll() {
                for (let dice of this.dices) {
                    if (dice.reroll === true) {
                        dice.diceValue = Math.floor(Math.random() * 6) + 1
                        this.showDice(dice)
                        dice.reroll = false
                        dice.activeDice = "inactiveDice"
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
                this.calculChiffres()
                this.calculPlusMoins()
                this.calculSuite()
                this.calculFull()
                this.calculCarre()
                this.calculYams()
                this.showCount()
                this.ptsCount = null
                this.selected = null
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
    border-radius: 20px;
}

#selector {
    margin: 10px;
}

#rollButton {
    padding: 5px;
}

#rollButton .buttons {
    margin: 5px;
}

</style>