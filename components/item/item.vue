<template>
    <div class="item">
        <img :src="imgItem" alt="">
    </div>
</template>

<script>
export default {
    props: {
        'imgItem': {
            type: String,
            default: null
        }
    }
}
</script>

<style scoped>
.item {
    width: 100%;
    height: 100%;
}
.img {
    width: 100%;
    height: auto;
}
</style>