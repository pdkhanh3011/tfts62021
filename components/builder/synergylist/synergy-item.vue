<template>
    <div v-if="synergiesSortByDes" class="synergy">
        <div v-for="(synergy, index) in synergiesSortByDes" :key="index" class="synergy-item">
            <div class="synergy-item-icon">
                <BuilderSynergylistSynergy-icon 
                :level-synergy="synergy.level"
                :img-synergy="getImgSynergy(synergy.name)"
                />
            </div>
            <div v-if="synergy.level != 0" class="synergy-item-count">
                <span>{{ synergy.count }}</span>
            </div>
            <div class="synergy-item-name-state">
                <h1 class="synergy-item-name">{{ synergy.name }}</h1>
                <div v-if="synergy.level >= 1" class="synergy-item-state">
                    <span v-for="(state,index1) in $options.dataSynergy.state.split('hehe')" :key="index1">
                        <span class="synergy-item-state-number"
                        :class="setActive(synergy.count, index1, $options.dataSynergy.state.split('hehe'))"
                        >{{ state }}</span>
                        <i class="fas fa-caret-right"></i>
                    </span>
                </div>
                <div v-if="synergy.level === 0" class="synergy-item-state">
                    <span>
                        <h1>{{synergy.count}} / {{$options.dataSynergy.state.split('hehe')[0]}}</h1>
                    </span>
                </div>
            </div>
            <div class="synergy-item-hover">
                <h1 class="synergy-item-hover-name">{{ $options.dataSynergy.name }}</h1>
                <h1 class="synergy-item-hover-description">{{ $options.dataSynergy.description }}</h1>
                <p v-for="(level,index1) in $options.dataSynergy.level.split('hehe')" 
                :key="index1" 
                :class="setActive(synergy.count, index1, $options.dataSynergy.state.split('hehe'))"
                >{{ level }}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        'synergiesSortByDes': {
            type: Array,
            default: null
        },
        'synergiesData': {
            type: Array,
            default: null
        }
    },
    dataSynergy: [],
    methods: {
        getImgSynergy(name) {
            this.$options.dataSynergy = this.synergiesData.find((a)=>a.name===name)
            return this.synergiesData.find((a)=>a.name===name).img_link
        },
        setActive(count, index, stateArr) {
            if (count === Number(stateArr[index])) {
                return "active"
            } else if (count === Number(stateArr[index])+1) {
                return "active"
            } else if (index === stateArr.length-1 && count > Number(stateArr[index])) {
                return "active"
            }
        }
    }
}
</script>

<style scoped>
.synergy {
    width: 100%;
    color: white;
    padding: 0 8px;
}
.synergy-item {
    width: 100%;
    display: flex;
    position: relative;
    align-items: center;
    font-size: 12px;
    background-color: #323232;
    padding: 8px;
    border: 1px solid black;
    border-radius: 5px;
    margin-bottom: 10px;
}
.synergy-item-icon {
    margin-right: 5px;
}
.synergy-item-state > span {
    margin-left: 5px;
}
.synergy-item-state span:last-child i {
    display: none;
}
.synergy-item-hover {
    position: absolute;
    display: none;
    left: 100%;
    top: 0;
    width: 300px;
    color: white;
    padding: 10px 10px;
    background-color: red;
    z-index: 10;
    pointer-events: none;
}
.synergy-item-hover .active {
    color: blue;
}
.synergy-item:hover .synergy-item-hover {
    display: block;
}
.synergy-item-hover-name {
    margin-bottom: 10px;
}
.synergy-item-hover-description {
    margin-bottom: 10px;
}
.synergy-item-count {
    margin-right: 8px;
}
.synergy-item-count span {
    display: inline-block;
    padding: 8px;
    border-radius: 3px;
    background-color: #646464;
}
.synergy-item-state > span:first-child {
    margin-left: 0;
}
.synergy-item-state {
    color: grey;
}
.synergy-item-state-number.active {
    color: white;
}
</style>