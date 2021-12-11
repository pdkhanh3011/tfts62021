<template>
    <div 
    draggable="true" 
    class="champion" 
    :class="cost()"
    @dragstart="onDragStart($event)"
    @dragend="onDragEnd()"
    >
        <img :src="championInfo.img_hero" alt="">
        <span class="price">${{championInfo.price}}</span>
        <span class="name">{{championInfo.name}}</span>
    </div>
</template>

<script>
export default {
    props: {
        'championInfo': {
            type:Object,
            default: null
        }
    },
    methods: {
        cost() {
            return `cost${this.championInfo.price}`
        },
        onDragStart(e) {
            e.dataTransfer.setData('name', this.championInfo.name)
            const builderitem = document.querySelectorAll('.builder-item')
            builderitem.forEach(element => {
                element.classList.add('on-start-drag')
            });
        },
        onDragEnd() {
            const builderitem = document.querySelectorAll('.builder-item')
            builderitem.forEach(element => {
                element.classList.remove('on-start-drag')
            });
        },
    }
}
</script>

<style scoped>
.champion {
    width: 100%;
    border-radius: 6px;
    overflow: hidden;
    position: relative;
}
img {
    width: 100%;
    height: auto;
    transform: scale(1.2);
}
.price, .name {
    position: absolute;
    text-shadow: -1px 0 2px #000, 0 1px 2px #000, 1px 0 2px #000, 0 -1px 2px #000;
    color: white;
    font-size: 11px;
}
.price {
    top: -1px;
    right: 0;
    padding: 1px 3px;
    line-height: 100%;
}
.name {
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    line-height: 16.5px;
}
.champion.cost1 {
    border: 2px solid gray;
}
.champion.cost1 .price {
    background-color: gray;
}
.champion.cost2 {
    border: 2px solid #11b288;
}
.champion.cost2 .price {
    background-color: #11b288;
}
.champion.cost3 {
    border: 2px solid #207ac7;
}
.champion.cost3 .price {
    background-color: #207ac7;
}
.champion.cost4 {
    border: 2px solid #c440da;
}
.champion.cost4 .price {
    background-color: #c440da;
}
.champion.cost5 {
    border: 2px solid #ffb93b;
}
.champion.cost5 .price {
    background-color: #ffb93b;
}
</style>

