<template>
    <main-screen v-if="statusMatch == 'default'" @onStart="onHandleBeforeStart($event)" />
    <interact-screen
        v-if="statusMatch == 'match'"
        :cardsContext="setting.cardsContext"
        @onFinish="onGetResult"
    />
    <result-screen v-if="statusMatch == 'result'" :timer="timer" @onStartAgain="statusMatch = 'default'" />
    <p class="copyright" v-if="statusMatch == 'default'">
        This game Flip Card Pokemon Memories using <a href="#">Vue3</a>
    </p>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import { shuffled } from "./utils/array";
import ResultScreen from "./components/ResultScreen.vue";

export default {
    name: "App",
    components: {
        MainScreen,
        InteractScreen,
        ResultScreen,
    },
    data() {
        return {
            setting: {
                totalOfBlocks: 0,
                cardsContext: [],
                startedAt: null,
            },
            statusMatch: "default",
            timer: 0,
        };
    },
    methods: {
        onHandleBeforeStart(config) {
            // console.log("Running handle before start: ", config);
            this.setting.totalOfBlocks = config.totalOfBlocks;

            const firstCards = Array.from({ length: this.setting.totalOfBlocks / 2 }, (_, i) => i + 1);
            const secondCards = [...firstCards];
            const cards = [...firstCards, ...secondCards];

            this.setting.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))));

            this.setting.startedAt = new Date().getTime();

            // Data ready
            this.statusMatch = "match";
        },
        onGetResult() {
            // get timer
            this.timer = new Date().getTime() - this.setting.startedAt;

            // switch to result component
            this.statusMatch = "result";
        },
    },
};
</script>

<style scoped>
.copyright {
    font-size: 1.5rem;
    position: fixed;
    bottom: 1.5rem;
    left: 50%;
    transform: translateX(-50%);
    z-index: 3;
    color: var(--light);
    letter-spacing: 2px;
}

.copyright a {
    color: #f4dc26;
}
</style>
