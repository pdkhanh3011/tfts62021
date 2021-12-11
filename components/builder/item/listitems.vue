<template>
	<div class="list-items-wrapper">
		<div v-for="(item,index) in itemsData" :key="index" class="list-item-equipment"
        draggable="true"
        @dragstart="onDragStart($event, item.name)"
        >
            <img :src="item.img_item" :alt="item.name">
        </div>
	</div>
</template>

<script>
export default {
	props: {
		itemsData: {
			type: Array,
			default: null,
		},
	},
    methods: {
        onDragStart(event, name) {
            event.dataTransfer.setData('nameItem', name)
        }
    }
}
</script>

<style scoped>
.list-items-wrapper {
    display: flex;
    flex-wrap: wrap;
}
.list-item-equipment {
    margin: 0 4px 4px 0;
}
.list-item-equipment > img {
    width: 32px;
    height: 32px;
    pointer-events: none;
}
</style>
