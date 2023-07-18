<template>
    <div class="screen">
        <div
            class="screen__inner"
            :style="{
                width: `${
                    ((((800 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 + 16) *
                    Math.sqrt(cardsContext.length)
                }px`,
            }"
        >
            <card-flip
                v-for="(card, index) in cardsContext"
                :key="index"
                :ref="`card-${index}`"
                :imgBackFaceUrl="`images/${card}.png`"
                :card="{ index, value: card }"
                :cardsContext="cardsContext"
                @onFlip="checkRule($event)"
            />
        </div>
    </div>
</template>

<script>
import CardFlip from "./Card.vue";

export default {
    props: {
        cardsContext: {
            type: Array,
            default: function () {
                return [];
            },
        },
    },
    components: {
        CardFlip,
    },
    data() {
        return {
            rules: [],
        };
    },
    methods: {
        checkRule(card) {
            if (this.rules.length == 2) return false;

            this.rules.push(card);

            if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
                console.log("Right ...");
                // add class disable to component card
                this.$refs[`card-${this.rules[0].index}`][0].onEnableDisableMode();
                this.$refs[`card-${this.rules[1].index}`][0].onEnableDisableMode();
                // reset array rules
                this.rules = [];

                const disableElements = document.querySelectorAll(".screen .card.disable");
                if (disableElements && disableElements.length === this.cardsContext.length - 2) {
                    setTimeout(() => {
                        this.$emit("onFinish");
                    }, 900);
                }
            } else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
                console.log("Wrong ...");
                setTimeout(() => {
                    // close two card
                    // console.log(this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard());
                    this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
                    this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
                    // reset array rules
                    this.rules = [];
                }, 800);
            } else {
                return false;
            }
        },
    },
};
</script>

<style scoped>
.screen {
    width: 100%;
    height: 100vh;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
    background: var(--dark);
    color: var(--light);
}

.screen__inner {
    display: flex;
    flex-wrap: wrap;
    margin: 2rem auto;
}
</style>
