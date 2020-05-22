<template>
    <div class="DialogCardsBlock">
        <button class="cw-dialogcards-prev cw-dialogcards-navbutton" @click="prevCard"></button>
        <div class="cw-dialogcards">
                <div class="scene scene--card" :class="[card.active ? 'active' : '']" v-for="card in cards" :key="card.index">
                    <div class="card" @click="flipCard">
                        <div class="card__face card__face--front">
                            <img v-if="card.front_img != '' && card.front_external_file == false" :src="coursewarePath+card.front_img" >
                            <img v-if="card.front_img != '' && card.front_external_file == true" :src="card.front_img" >
                            <div v-if="card.front_img == ''" class="cw-dialogcards-front-no-image"></div>
                            <p>{{card.front_text}}</p>
                        </div>
                        <div class="card__face card__face--back">
                            <img v-if="card.back_img != '' && card.back_external_file == false" :src="coursewarePath+card.back_img">
                            <img v-if="card.back_img != '' && card.back_external_file == true" :src="card.back_img">
                            <div v-if="card.back_img == ''" class="cw-dialogcards-back-no-image"></div>
                            <p>{{card.back_text}}</p>
                        </div>
                    </div>
                </div>
        </div>
        <button class="cw-dialogcards-next cw-dialogcards-navbutton" @click="nextCard"></button>
  </div>
</template>

<script>
export default {
    name: 'DialogCardsBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    data(){
        return {
            cards: []
        }
    },
    beforeMount() {
        this.cards = this.block.data.dialogcards_content;
        this.cards.forEach((card, index) => {
            if (index == 0) {
                card.active = true;
            } else {
                card.active = false;
            }
        });
    },
    methods: {
        flipCard(event) {
            event.currentTarget.classList.toggle('is-flipped');
        },
        nextCard() {
            let view = this;
            let cards = this.cards;
            this.cards = [];
            cards.every((card, index) => {
                if (card.active) {
                    if(cards.length > index + 1) {
                        card.active = false;
                        cards[index + 1].active = true;
                    }
                    return false; // end every
                } else {
                    return true; // continue every
                }
            });

            this.cards= cards;
        },
        prevCard() {
            let view = this;
            let cards = this.cards;
            this.cards = [];
            cards.every((card, index) => {
                if (card.active) {
                    if (index > 0) {
                        card.active = false;
                        cards[index - 1].active = true;
                    }
                    return false; // end every
                } else {
                    return true; // continue every
                }
            });

            this.cards= cards;
        }

    }
}
</script>

<style lang="less">
.DialogCardsBlock {
    display: flex;
    .cw-dialogcards {
        flex: auto;
        .scene {
            margin: 0 auto;
            width: 440px;
            height: 520px;
            perspective: 880px;
            display: none;
            &.active {
                display: block;
                animation: shake 0.82s cubic-bezier(.36,.07,.19,.97) both;
                transform: translate3d(0, 0, 0);
                backface-visibility: hidden;
                perspective: 1000px;
            } 
        }

        .card {
            width: 100%;
            height: 100%;
            transition: transform 1s;
            transform-style: preserve-3d;
            cursor: pointer;
            position: relative;
        }

        .card.is-flipped {
            transform: rotateY(180deg);
        }

        .card__face {
            position: absolute;
            width: 100%;
            height: 100%;
            color: #000;
            text-align: center;
            font-weight: bold;
            font-size: 1.2em;
            backface-visibility: hidden;
            box-shadow: 0 2px 15px rgba(0,0,0,0.30);

            img {
                max-width: 380px;
                max-height: 220px;
                margin-top: 1em;
            }

            .cw-dialogcards-front-no-image {
                background-image: url("../assets/icons/lightblue/question.svg");
            }

            .cw-dialogcards-back-no-image {
                background-image: url("../assets/icons/lightblue/exclaim.svg");
            }

            .cw-dialogcards-front-no-image,
            .cw-dialogcards-back-no-image {
                width: 100%;
                height: 180px;
                margin-top: 2em;
                background-repeat: no-repeat;
                background-size: 150px 150px;
                background-position-x: center;
            }

            p {
                margin: 1em 3em 1em 4em;
                padding-right: 1em;
                overflow-y: auto;
                max-height: 12em;
                text-align: justify;
            }
        }

        .card__face--front {
            background-color: #fff;
            background-image: url("../assets/icons/blue/arr_1right.svg");
            background-repeat: no-repeat;
            background-position: 95% 95%;
        }

        .card__face--back {
            background-color: #fff;
            background-image: url("../assets/icons/blue/arr_1left.svg");
            background-repeat: no-repeat;
            background-position: 5% 95%;
            transform: rotateY(180deg);
        }

    }

    .cw-dialogcards-navbutton {
        color: transparent;
        width: 35px;
        height: 35px;
        background-color: #bbb;
        background-size: 20px;
        border-radius: 2px;
        background-size: 24px;
        background-position: 50%;
        background-repeat: no-repeat;
        background-color: #28497c;
        border: none;
        outline: none;
        display: block;
        z-index: 42;
        margin: auto;
        padding: 0;
        cursor: pointer;

        &.cw-dialogcards-prev {
            background-image: url("../assets/icons/white/arr_1left.svg");
        }

        &.cw-dialogcards-next {
            background-image: url("../assets/icons/white/arr_1right.svg");
            right: 0;
        }
    }

    @keyframes shake {
        10%, 90% {
            transform: translate3d(-1px, 0, 0);
        }
        
        20%, 80% {
            transform: translate3d(2px, 0, 0);
        }

        30%, 50%, 70% {
            transform: translate3d(-4px, 0, 0);
        }

        40%, 60% {
            transform: translate3d(4px, 0, 0);
        }
    }
}
</style>
