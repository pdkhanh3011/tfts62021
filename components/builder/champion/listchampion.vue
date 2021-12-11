<template>
    <div class="list-champion"
    @dragover.prevent=""
    @drop="removeChampion($event)"
    >
        <div class="filter">
            <div class="name" @click="sortByName()">Name</div>
            <div class="cost" @click="sortByPrice()">Cost</div>
        </div>
        <div class="list">
            <div v-for="(champion,index) in championData" :key="index" class="item">
                <champion :champion-info="champion"
                />
            </div>
        </div>
    </div>
</template>

<script>
import champion from "~/components/builder/champion/champion-avatar.vue"
export default {
    components: {champion},
    props: {
        'championData': {
            type: Array,
            default: null
        }
    },
    methods: {
        sortByPrice() {
            // eslint-disable-next-line vue/no-mutating-props
            this.data = this.championData.sort(function (a, b) {
                return a.price - b.price;
            });
        },
        sortByName() {
            // eslint-disable-next-line vue/no-mutating-props
            this.data = this.championData.sort(function(a, b) {
            const nameA = a.name.toUpperCase();
            const nameB = b.name.toUpperCase();
            if (nameA < nameB) {
                return -1;
            }
            if (nameA > nameB) {
                return 1;
            }
            return 0;
            });
        },
        removeChampion(e) {
			const nameInBoard = e.dataTransfer.getData('nameInBoard')
            const positionInBoard = e.dataTransfer.getData('positionInBoard')
            this.$emit('remove-champion', {
                nameInBoard,
                positionInBoard
            })
        }
    }
}
</script>

<style scoped>
.origin {
    color: white;
}
.listchampion {
    width: 100%;
}
.filter {
    display: flex;
    margin-bottom: 15px;
}
.filter div {
    background-color: #6c757d;
    border: 1px solid #6c757d;
    border-radius: 3px;
    padding: 4px 8px;
    font-size: 12px;
    color: white;
    margin-left: 10px;
    cursor: pointer;
}
.filter div:first-child {
    margin-left: 0;
}
.list {
    display: flex;
    flex-wrap: wrap;
    padding-top: 5px;
    padding-left: 5px;
    padding-bottom: 2px;
}
.list.drop {
    background-color: rebeccapurple;
}
.item {
    width: 66.6px;
    margin-right: 4px;
    margin-bottom: 4px;
    cursor: pointer;
}
.item:nth-child(10) {
    margin-right: 0;
}
</style>