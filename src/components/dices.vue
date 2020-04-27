<template>
    <div id="dices">
        <b-container>
            <p><strong>Player {{player}} turn</strong></p>
            <p><strong>{{tries}}</strong> {{triesText}} left.</p>
            <div id="dicesImages">
                <img :src="dices[0].diceImg" :class="dices[0].activeDice" @click="rerollDice(dices[0])">
                <img :src="dices[1].diceImg" :class="dices[1].activeDice" @click="rerollDice(dices[1])">
                <img :src="dices[2].diceImg" :class="dices[2].activeDice" @click="rerollDice(dices[2])">
                <img :src="dices[3].diceImg" :class="dices[3].activeDice" @click="rerollDice(dices[3])">
                <img :src="dices[4].diceImg" :class="dices[4].activeDice" @click="rerollDice(dices[4])">
            </div>
            <div id="selector"> 
                <p v-if="!selected && tries === 3"> <strong>Player {{player}}: Roll the dices !! </strong></p>
                <p v-if="!selected && tries < 3 && tries > 0"> <strong>Player {{player}}: Roll again or select a combination to submit ({{tries}} {{triesText}} left.) </strong></p>
                <p v-if="!selected && tries === 0"> <strong>Player {{player}}: Select a combination to submit.</strong></p>
                <p v-if="selected && tries === 0 && submitted===false"> <strong>Player {{player}}: Choose a combination and press Submit. Your score will be added to the sheet.</strong></p>
                <p v-if="selected && tries > 0 && submitted===false"> <strong>Player {{player}}: Roll again or submit this combination ({{tries}} {{triesText}} left.).</strong></p>
                <p v-if="pressNext && submitted"> <strong> Press "Next Player" button</strong></p> 
                <b-select v-model="selected" :options="combinations" @input="showCount"></b-select>
                <div class="mt-3">Selected: <strong>{{ selected }} ( {{ptsCount}} points )</strong></div>
            </div>
            <div id="rollButton">
                <b-button class="buttons" @click="roll" :disabled="disableRoll">Roll</b-button>
                <b-button class="buttons" @click="submitCombination" :disabled="disableSubmit">Submit</b-button>
                <b-button class="buttons" @click="nextPlayer" :disabled="disableNextPlayer">Next Player</b-button>
            </div>
            <div id="scoreSheet">
                <div>
                    <b-table :items="turns" :fields="sheetFields"></b-table>
                </div>
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
                disableSubmit : true,
                pressNext : false,
                submitted : false,
                disableNextPlayer : true,
                count : 0,
                ptsCount: null,
                brelan : "false",
                selected: null,
                combinations: [
                    { value: null, text: 'Select a combination'},
                    { disabled: true, value: 'One', text: 'One', points: 0, p1: false, p2: false},
                    { disabled: true, value: 'Two', text: 'Two', points: 0, p1: false, p2:false},
                    { disabled: true, value: 'Three', text: 'Three', points: 0, p1: false, p2:false},
                    { disabled: true, value: 'Four', text: 'Four', points: 0, p1: false, p2:false},
                    { disabled: true, value: 'Five', text: 'Five', points: 0, p1: false, p2:false},
                    { disabled: true, value: 'Six', text: 'Six', points: 0, p1: false, p2:false},
                    { disabled: true, value: 'Plus', text: 'Plus', points: 0, p1: false, p2:false},
                    { disabled: true, value: 'Minus', text: 'Minus', points:0, p1: false, p2:false},
                    { disabled: true, value: 'Straight', text: 'Straight', points:0, p1: false, p2:false},
                    { disabled: true, value: 'Full', text: 'Full', points:0, p1: false, p2:false},
                    { disabled: true, value: '4 of a kind', text: '4 of a kind', points:0, p1: false, p2:false},
                    { disabled: true, value: "Yam's", text: "Yam's", points: 0, p1: false, p2:false},
                ],
                sheetFields: ['combination', 'player_1', 'player_2'],
                turns: [
                    { isActive : false, combination: "1", player_1 : null, player_2 : null },
                    { isActive : false, combination: "2", player_1 : null, player_2 : null },
                    { isActive : false, combination: "3", player_1 : null, player_2 : null },
                    { isActive : false, combination: "4", player_1 : null, player_2 : null },
                    { isActive : false, combination: "5", player_1 : null, player_2 : null },
                    { isActive : false, combination: "6", player_1 : null, player_2 : null },
                    { _rowVariant: 'success', isActive : false, combination: "Total I", player_1 : 0, player_2 : 0},
                    { _rowVariant: 'success', isActive : false, combination: "Bonus (T>=63)", player_1 : 0, player_2 : 0},
                    { isActive : false, combination: "Plus", player_1 : null, player_2 : null },
                    { isActive : false, combination: "Minus", player_1 : null, player_2 : null },
                    { _rowVariant: 'success', isActive : false, combination: "Total II", player_1 : 0, player_2 : 0},
                    { isActive : false, combination: "Straight", player_1 : null, player_2 : null },
                    { isActive : false, combination: "Full", player_1 : null, player_2 : null },
                    { isActive : false, combination: "4 OAK", player_1 : null, player_2 : null },
                    { isActive : false, combination: "Yam's", player_1 : null, player_2 : null },
                    { _rowVariant: 'success', isActive : false, combination: "Total III", player_1 : 0, player_2 : 0},
                    { _rowVariant: 'success', isActive : false, combination: "Total I+II+III", player_1 : 0, player_2 : 0},
                ]
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
            infoSubmit() {
                for (let turn of this.turns) {
                    if ( turn.isActive == true) {
                        turn._rowVariant = "info"
                    } else {
                        turn._rowVariant = null
                    }
                } 
                this.turns[6]._rowVariant ="success"
                this.turns[7]._rowVariant ="success"
                this.turns[10]._rowVariant ="success"
                this.turns[15]._rowVariant ="success"
                this.turns[16]._rowVariant ="success"
            },
            nextPlayer() {
                this.submitted = false
                this.pressNext = false
                this.tries = 3
                this.triesText = "tries"
                this.ptsCount = null
                this.selected = null
                for (let turn of this.turns) {
                    turn.isActive = false
                }
                this.infoSubmit()
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
                    this.selected = "Straight"
                } else if ( this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 && this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 && this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 && this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 && this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 1 ) {
                    this.combinations[9].points = 20
                    this.selected = "Straight"
                }
            },
            calculFull() {
                if ( this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 3 && (this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 )) {
                    this.combinations[10].points = 30
                    this.selected = "Full"
                } else if ( this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 3 && (this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 )) {
                    this.combinations[10].points = 30
                    this.selected = "Full"
                } else if ( this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 3 && (this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 )) {
                    this.combinations[10].points = 30
                    this.selected = "Full"
                } else if ( this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 3 && (this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 )) {
                    this.combinations[10].points = 30
                    this.selected = "Full"
                } else if ( this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 3 && (this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 )) {
                    this.combinations[10].points = 30
                    this.selected = "Full"
                } else if ( this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 3 && (this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 || this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 2 )) {
                    this.combinations[10].points = 30
                    this.selected = "Full"
                }
            },
            calculCarre() {
                if ( this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 4 || this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 4 || this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 4 || this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 4 || this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 4 || this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) >= 4 ) {
                    this.combinations[11].points = 40
                    this.selected= "4 of a kind"
                }
            },
            calculYams() {
                if ( this.counter(1, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 5 || this.counter(2, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 5 || this.counter(3, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 5 || this.counter(4, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 5 || this.counter(5, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 5 || this.counter(6, this.dices[0].diceValue, this.dices[1].diceValue, this.dices[2].diceValue, this.dices[3].diceValue, this.dices[4].diceValue) == 5 ) {
                    this.combinations[10].points = 30
                    this.combinations[11].points = 40
                    this.combinations[12].points = 50
                    this.selected = "Yam's"
                }
            },
            showCount() {
                if (this.tries < 3) {
                    switch (this.selected) {
                        case "One":
                            this.ptsCount = this.combinations[1].points
                            this.disableSubmit = false
                            break;
                        case "Two":
                            this.ptsCount = this.combinations[2].points
                            this.disableSubmit = false
                            break;
                        case "Three":
                            this.ptsCount = this.combinations[3].points
                            this.disableSubmit = false
                            break;
                        case "Four":
                            this.ptsCount = this.combinations[4].points
                            this.disableSubmit = false
                            break;
                        case "Five":
                            this.ptsCount = this.combinations[5].points
                            this.disableSubmit = false
                            break;
                        case "Six":
                            this.ptsCount = this.combinations[6].points
                            this.disableSubmit = false
                            break;
                        case "Plus":
                            this.ptsCount = this.combinations[7].points
                            this.disableSubmit = false
                            break;
                        case "Minus":
                            this.ptsCount = this.combinations[8].points
                            this.disableSubmit = false
                            break;
                        case "Straight":
                            this.ptsCount = this.combinations[9].points
                            this.disableSubmit = false
                            break;
                        case "Full":
                            this.ptsCount = this.combinations[10].points
                            this.disableSubmit = false
                            break;
                        case "4 of a kind":
                            this.ptsCount = this.combinations[11].points
                            this.disableSubmit = false
                            break;
                        case "Yam's":
                            this.ptsCount = this.combinations[12].points
                            this.disableSubmit = false
                            break;
                        default:
                            this.ptsCount = null
                            this.disableSubmit = true
                    }
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
                this.calculChiffres()
                this.calculPlusMoins()
                this.calculSuite()
                this.calculFull()
                this.calculCarre()
                this.calculYams()
                this.showCount()
                this.ptsCount = null
                if (this.player == 1) {
                    for (let combination of this.combinations) {
                        if (combination.p1 === true) {
                            combination.disabled = true
                        } else {
                        combination.disabled = false
                        }
                    }
                } else if (this.player == 2) {
                    for (let combination of this.combinations) {
                        if (combination.p2 === true) {
                            combination.disabled = true
                        } else combination.disabled = false
                    }
                }
            },
            submitCombination () {
                if (this.player == 1) {
                    switch (this.selected) {
                        case "One":
                            this.turns[0].isActive = true
                            this.turns[0].player_1 = this.combinations[1].points
                            this.combinations[1].p1 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Two":
                            this.turns[1].isActive = true
                            this.turns[1].player_1 = this.combinations[2].points
                            this.combinations[2].p1 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Three":
                            this.turns[2].isActive = true
                            this.turns[2].player_1 = this.combinations[3].points
                            this.combinations[3].p1 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Four":
                            this.turns[3].isActive = true
                            this.turns[3].player_1 = this.combinations[4].points
                            this.combinations[4].p1 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Five":
                            this.turns[4].isActive = true
                            this.turns[4].player_1 = this.combinations[5].points
                            this.combinations[5].p1 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Six":
                            this.turns[5].isActive = true
                            this.turns[5].player_1 = this.combinations[6].points
                            this.combinations[6].p1 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Plus":
                            this.turns[8].isActive = true
                            this.turns[8].player_1 = this.combinations[7].points
                            this.combinations[7].p1 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Minus":
                            this.turns[9].isActive = true
                            this.turns[9].player_1 = this.combinations[8].points
                            this.combinations[8].p1 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Straight":
                            this.turns[11].isActive = true
                            this.turns[11].player_1 = this.combinations[9].points
                            this.combinations[9].p1 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Full":
                            this.turns[12].isActive = true
                            this.turns[12].player_1 = this.combinations[10].points
                            this.combinations[10].p1 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "4 of a kind":
                            this.turns[13].isActive = true
                            this.turns[13].player_1 = this.combinations[11].points
                            this.combinations[11].p1 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Yam's":
                            this.turns[14].isActive = true
                            this.turns[14].player_1 = this.combinations[12].points
                            this.combinations[12].p1 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        default:
                            this.ptsCount = null
                            this.disableSubmit = true
                    }
                } else if (this.player == 2) {
                    switch (this.selected) {
                        case "One":
                            this.turns[0].isActive = true
                            this.turns[0].player_2 = this.combinations[1].points
                            this.combinations[1].p2 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Two":
                            this.turns[1].isActive = true
                            this.turns[1].player_2 = this.combinations[2].points
                            this.combinations[2].p2 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Three":
                            this.turns[2].isActive = true
                            this.turns[2].player_2 = this.combinations[3].points
                            this.combinations[3].p2 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Four":
                            this.turns[3].isActive = true
                            this.turns[3].player_2 = this.combinations[4].points
                            this.combinations[4].p2 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Five":
                            this.turns[4].isActive = true
                            this.turns[4].player_2 = this.combinations[5].points
                            this.combinations[5].p2 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Six":
                            this.turns[5].isActive = true
                            this.turns[5].player_2 = this.combinations[6].points
                            this.combinations[6].p2 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Plus":
                            this.turns[8].isActive = true
                            this.turns[8].player_2 = this.combinations[7].points
                            this.combinations[7].p2 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Minus":
                            this.turns[9].isActive = true
                            this.turns[9].player_2 = this.combinations[8].points
                            this.combinations[8].p2 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Straight":
                            this.turns[11].isActive = true
                            this.turns[11].player_2 = this.combinations[9].points
                            this.combinations[9].p2 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Full":
                            this.turns[12].isActive = true
                            this.turns[12].player_2 = this.combinations[10].points
                            this.combinations[10].p2 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "4 of a kind":
                            this.turns[13].isActive = true
                            this.turns[13].player_2 = this.combinations[11].points
                            this.combinations[11].p2 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        case "Yam's":
                            this.turns[14].isActive = true
                            this.turns[14].player_2 = this.combinations[12].points
                            this.combinations[12].p2 = true
                            this.disableSubmit = true
                            this.disableNextPlayer = false
                            break;
                        default:
                            this.ptsCount = null
                            this.disableSubmit = true
                    }  
                }
                this.disableRoll = true
                this.infoSubmit()
                this.turns[6].player_1 = this.turns[0].player_1 + this.turns[1].player_1 + this.turns[2].player_1 + this.turns[3].player_1 + this.turns[4].player_1 + this.turns[5].player_1
                if (this.turns[6].player_1 >= 63) {this.turns[7].player_1 = 35}
                this.turns[6].player_2 = this.turns[0].player_2 + this.turns[1].player_2 + this.turns[2].player_2 + this.turns[3].player_2 + this.turns[4].player_2 + this.turns[5].player_2
                if (this.turns[6].player_2 >= 63) {this.turns[7].player_2 = 35}
                this.turns[10].player_1 = this.turns[8].player_1 - this.turns[9].player_1
                this.turns[10].player_2 = this.turns[8].player_2 - this.turns[9].player_2
                this.turns[15].player_1 = this.turns[11].player_1 + this.turns[12].player_1 + this.turns[13].player_1 + this.turns[14].player_1
                this.turns[15].player_2 = this.turns[11].player_2 + this.turns[12].player_2 + this.turns[13].player_2 + this.turns[14].player_2
                this.turns[16].player_1 = this.turns[6].player_1 + this.turns[7].player_1 + this.turns[10].player_1 + this.turns[15].player_1
                this.turns[16].player_2 = this.turns[6].player_2 + this.turns[7].player_2 + this.turns[10].player_2 + this.turns[15].player_2 
                this.submitted = true
                this.pressNext = true
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

#scoreSheet {
    margin: 10px;
}

</style>
