<template>
    <div class="synergy-item-icon-container" :class="`level${levelSynergy}`">
        <img :src="imgSynergy">
    </div>
</template>

<script>
export default {
    props: {
        'levelSynergy': {
            type: Number,
            default: null
        },
        'imgSynergy': {
            type: String,
            default: null
        },
    },
}
</script>

<style scoped>
.synergy-item-icon-container {
    clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
    background-repeat: no-repeat;
    width: 30px;
    height: 32px;
    display: flex;
    justify-content: center;
    align-items: center;
} 
.synergy-item-icon-container.level0 {
    background-image: url(https://lolchess.gg/images/tft/traits/background/darken.svg);
}
.synergy-item-icon-container.level1 {
    background-image: url(https://lolchess.gg/images/tft/traits/background/bronze.svg);
}
.synergy-item-icon-container.level2 {
    background-image: url(https://lolchess.gg/images/tft/traits/background/silver.svg);
}
.synergy-item-icon-container.level3 {
    background-image: url(https://lolchess.gg/images/tft/traits/background/gold.svg);
}
.synergy-item-icon-container.level4 {
    background-image: url(https://lolchess.gg/images/tft/traits/background/chromatic.svg);
}
img {
    width: 16px;
    height: 16px;
    display: block;
    -webkit-filter: invert(100%); /* Safari/Chrome */
    filter: invert(100%);
}
</style>