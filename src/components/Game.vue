<template>
    <div>
        <div class="d-flex flex-row justify-content-center py-3">
            <div class="turns p-3"><span class="btn btn-secondary">Turns : <span class="badge" :class="isFinish ? 'badge-success' : 'badge-light'">{{turns}}</span> </span></div>
            <div class="totalTime p-3"><span class="btn btn-secondary">Total Time : <span class="badge" :class="isFinish ? 'badge-success' : 'badge-light'">{{min}} : {{sec}}</span></span></div>
            <div class="totalTime p-3"><button class="btn btn-dark" @click="resetGame" :disabled="!isStart">Restart</button></div>
        </div>
        <div class="row">
            <div class="col-md-12 col-lg-12 col-xl-10 mx-auto">
                <div class="row justify-content-md-center">
                    <div v-for="card in memoryCards" 
                        v-bind:key="card.id"
                        class="col-auto mb-3 flip-container"
                         :class="{ 'flipped': card.isFlipped, 'matched' : card.isMatched }"
                         @click="flipCard(card)"
                    >
                        <div class="memorycard">
                            <div class="front border rounded shadow"><img width="125" height="150" src="@/assets/images/pattern.png"></div>
                            <div class="back rounded border"><img width="125" height="150" :src="require(`@/assets/images/${card.img}`)"></div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Vue from 'vue'
import _ from 'lodash';

export default {
    name: 'Game',
    props: ['cardData'],
    data() {
        return {
            cards: this.cardData,
            memoryCards: [],
            flippedCards: [],
            isFinish: false,
            isStart: false,
            turns: 0,
            totalTime: {
                minutes: 0,
                seconds: 0,
            },
        }
    },
    methods: {
        flipCard(card){
            if(card.isMatched || card.isFlipped || this.flippedCards.length === 2)
                return;

            if(!this.isStart){
                this.startGame();
            }

            card.isFlipped = true;

            if(this.flippedCards.length < 2)
                this.flippedCards.push(card);
            if(this.flippedCards.length === 2)
                this.match(card);
        },

        // eslint-disable-next-line no-unused-vars
        match(card){
            this.turns++;

            if(this.flippedCards[0].name === this.flippedCards[1].name){
                setTimeout(() => {
                    this.flippedCards.forEach(card => card.isMatched = true);
                    this.flippedCards = [];

                    //All cards matched ?
                    if(this.memoryCards.every(card => card.isMatched === true)){
                        clearInterval(this.interval);
                        this.isFinish = true;
                    }

                }, 400);
            }
            else{
                setTimeout(() => {
                    this.flippedCards.forEach(card => card.isFlipped = false);
                    this.flippedCards = [];
                }, 600);
            }
        },

        startGame() {
            this.tick();
            this.interval = setInterval(this.tick,1000);
            this.isStart = true;
        },

        tick(){
            if(this.totalTime.seconds !== 59){
                this.totalTime.seconds++;
                return
            }

            this.totalTime.minutes++;
            this.totalTime.seconds = 0;
        },

        resetGame(){
            this.$emit('handleResetGame');
        },
    },
    computed:{
        sec(){
            if(this.totalTime.seconds < 10){
                return '0'+this.totalTime.seconds;
            }
            return this.totalTime.seconds;
        },
        min(){
            if(this.totalTime.minutes < 10){
                return '0'+this.totalTime.minutes;
            }
            return this.totalTime.minutes;
        }
    },
    created(){
        clearInterval(this.interval);

        this.cards.forEach((card) => {
            Vue.set(card, 'isFlipped',false);
            Vue.set(card, 'isMatched',false);
        });

        setTimeout(() => {
            this.memoryCards = [];
            this.memoryCards = _.shuffle(this.memoryCards.concat(_.cloneDeep(this.cards), _.cloneDeep(this.cards)));
            this.totalTime.minutes = 0;
            this.totalTime.seconds = 0;
            this.isStart = false;
            this.isFinish = false;
            this.turns = 0;
            this.flippedCards = [];
        }, 500);
    },
}
</script>

<style>

</style>