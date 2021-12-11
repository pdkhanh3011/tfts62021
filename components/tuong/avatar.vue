<template>
    <div class="avatar"  
    :style="{ width: width + 'px', height: height + 'px' }" 
    :class="borderColor"
    @click="$emit('click-avatar')"
    >
        <span v-if="level === 1" class="star one-star">★</span>
        <span v-if="level === 2" class="star two-star">★★</span>
        <span v-if="level === 3" class="star three-star">★★★</span>
        <div class="avatar-item">
            <div v-for="(itemImg,index) in itemList" :key="index" class="item-container">
                <item :img-item="itemImg"/>
            </div>
        </div>
        <img class="champion-avatar" :src="championInfo.img_hero" :alt="championInfo.name">
        <div class="champion-description">
            <div class="header">
                <div class="header-name">
                    <h1>{{ championInfo.name }}</h1>
                </div>
                <div class="header-price">
                    <i class="fas fa-coins header-price-icon"></i>
                    <span>{{ championInfo.price }}</span>
                </div>
            </div>
            <div class="content">
                <div class="synergy">
                    <div v-for="(item,index) in arrSynergy" 
                    :key="index" 
                    class="synergy-item">
                        <img :src="imgSynergy(item)" alt="">
                        <span>{{ item }}</span>
                    </div>
                </div>
                <div class="skill">
                    <div class="img-name-mana">
                        <div class="skill-img">
                            <img :src="championInfo.img_skill_link" alt="">
                        </div>
                        <div class="name-mana">
                            <h1 class="name">{{ championInfo.skillname }}</h1>
                            <h1 class="mana">{{ championInfo.mana }}</h1>
                        </div>
                    </div>
                    <div class="skill-description">
                        <p>{{ championInfo.skilldescription }}</p>
                    </div>
                    <div class="skill-levelup">
                        <h1 v-for="(item,index) in skillLevel" :key="index">
                            {{ item }}</h1>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import item from "~/components/item/item.vue"
export default {
    components: { item },
    props: {
        'width': {
            type: Number,
            default: null
        },
        'height': {
            type: Number,
            default: null
        },
        'level': {
            type: Number,
            default: null
        },
        'itemList': {
            type: Array,
            default: null
        },
        'championInfo': {
            type: Object,
            default: null
        },
        'synergyData': {
            type: Array,
            default: null
        }
    },
    computed: {
        // eslint-disable-next-line vue/return-in-computed-property
        borderColor() {
            switch (this.championInfo.price) {
                case 1: return "cost-1"
                case 2: return "cost-2"
                case 3: return "cost-3"
                case 4: return "cost-4"
                case 5: return "cost-5"
            }
        },
        arrSynergy() {
            return (this.championInfo.he + "," + this.championInfo.toc).split(",")
        },
        skillLevel() {
            return this.championInfo.skilldetails.split(",")
        }
    },
    methods: {
        imgSynergy(a) {
            return this.synergyData.find((item)=>item.name === a).img_link
        }
    }
}
</script>

<style scoped>
.avatar {
    position: relative;
    cursor: pointer;
}
.avatar.cost-1 {
    border: 2px solid #808080;
}
.avatar.cost-2{
    border: 2px solid rgb(17, 178, 136);
}
.avatar.cost-3{
    border: 2px solid rgb(32, 122, 199);
}
.avatar.cost-4{
    border: 2px solid rgb(196, 64, 218);
}
.avatar.cost-5{
    border: 2px solid rgb(255, 185, 59);
}
img {
    width: 100%;
    height: auto;
}
.star {
    position: absolute;
    top: -60%;
    left: 50%;
    transform: translateX(-50%);
    font-size: 13px;
    line-height: 100%;
}
.one-star {
    color: rgb(194, 212, 248);
}
.two-star {
    color: rgb(194, 212, 248);
}
.three-star {
    color: rgb(228, 193, 87);
}
.avatar-item {
    position: absolute;
    display: flex;
    bottom: -26%;
    left: 50%;
    transform: translateX(-50%);
}
.item-container {
    width: 12px;
    height: 12px;
}
.champion-description {
    visibility: hidden;
    position: absolute;
    width: 300px;
    background-color: rgba(0, 0, 0, .85);
    top: 50px;
    left: -130px;
    opacity: 0;
    transition: all 0.2s;
    z-index: 1;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
}
.champion-avatar:hover + .champion-description {
    visibility: visible;
    opacity: 1;
    top: 60px;
}
.header {
    border-bottom: 1px solid white;
    display: flex;
    justify-content: space-between;
    padding: 7px 15px;
}
.header-name h1 {
    color: #3dd0d8;
    font-size: 16px;
    font-weight: 600;
}
.header-price span {
    font-size: 16px;
    color: white;
}
.content {
    color: white;
    padding: 5px 15px 15px;
    margin-top: 2px;
}
.synergy {
    margin-bottom: 12px;
}
.synergy-item img {
    width: 20px;
    height: 20px;
    display: inline-block;
}
.synergy-item span {
    font-size: 14px;
    color: #80AFDC;
    font-weight: 600;
    vertical-align: middle;
}
.img-name-mana {
    display: flex;
    align-items: center;
    margin-bottom: 9px;
}
.skill-img {
    width: 25px;
    height: 25px;
}
.name-mana h1 {
    font-weight: 600;
}
.name-mana {
    margin-left: 10px;
}
.name {
    color: #80afdc;
    font-size: 12px;
}
.mana {
    color: white;
    font-size: 11px;
}
.skill-description {
    font-size: 12px;
    color: #8ab3dd;
    margin-bottom: 3px;
}
.header-price-icon {
    color: rgb(255, 185, 59);
}
.skill-levelup h1 {
    font-size: 12px;
    color: white;
    font-weight: 600;
}
</style>