<template>
    <div class="screen">
        <h1>POKEMON MEMORIES</h1>
        <h3>Select mode to start game</h3>
        <div class="actions">
            <button @click="onStart(16)">
                <span>4x4</span>
                <span>Easy</span>
            </button>
            <button @click="onStart(36)">
                <span>6x6</span>
                <span>Normal</span>
            </button>
            <button @click="onStart(64)">
                <span>8x8</span>
                <span>Hard</span>
            </button>
            <button @click="onStart(100)">
                <span>10x10</span>
                <span>Supper Hard</span>
            </button>
        </div>
    </div>
</template>

<script>
export default {
    methods: {
        onStart(totalOfBlocks) {
            this.$emit("onStart", { totalOfBlocks });
        },
    },
};
</script>

<style scoped>
.screen {
    width: 100%;
    height: 100vh;
    background: var(--dark);
    color: var(--light);
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
    display: flex;
    align-content: center;
    justify-content: center;
    flex-direction: column;
    flex-wrap: wrap;
}

.screen h1 {
    font-size: 4.5rem;
    letter-spacing: 2px;
    text-transform: uppercase;
    margin: 0 auto;
}

.screen h3 {
    font-size: 2rem;
    letter-spacing: 2px;
    margin: 0 auto;
}

.actions {
    display: flex;
    margin-top: 2rem;
}

.actions button {
    font: var(--font);
    width: 150px;
    height: 150px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin: 0 1rem;
    color: var(--light);
    background: transparent;
    box-shadow: none;
    border: 1px solid var(--light);
    border-radius: 1rem;
    cursor: pointer;
    transition: background 0.3s ease-in-out;
}

.actions button:hover {
    background-color: var(--light);
    color: var(--dark);
}

.actions button span:first-child {
    font-size: 2rem;
}

.actions button span:last-child {
    display: block;
    font-size: 1.25rem;
    margin-top: 0.5rem;
}
</style>
