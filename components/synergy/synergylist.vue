<template>
	<div class="container">
		<div
			v-for="(item, index) in synergyList"
			:key="index"
			class="synergy"
			:class="levelClass(item[0].state, item.count)"
		>
			<div class="item-img">
				<img
					:src="`//lolchess.gg/images/tft/traiticons-white/6.0/${item[0].name}.svg`"
				/>
			</div>
			<div class="item-number">
				<span>{{ item.count }}</span>
			</div>
			<div class="item-description">
				<!-- <p </p> -->
				<div class="item-description-name">
					<h1>{{ item[0].name }}</h1>
				</div>
				<div class="item-description-title">
					<p>
						{{ item[0].description }}
					</p>
				</div>
				<div class="item-description-level">
					<h1
						v-for="(level, index2) in item[0].level.split('hehe')"
						:key="index2"
						:class="addActive(item.count, level)"
					>
						- {{ level }}
					</h1>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	props: {
		synergyList: {
			type: Array,
			default: null,
		},
	},
	methods: {
		levelClass(state, count) {
			let total = 0
			const arr = state.split('hehe')
			if (state.length === 1) {
				for (const item in arr) {
					if (count >= arr[item]) {
						total = total + 3
					}
				}
			} else if (state.length === 2) {
				for (const item in arr) {
					if (count >= arr[item]) {
						total = total + 2
					}
				}
			} else {
				for (const item in arr) {
					if (count >= arr[item]) {
						total = total + 1
					}
				}
			}
			switch (total) {
				case 0:
					return 'hide'
				case 1:
					return 'bronze'
				case 2:
					return 'silver'
				case 3:
					return 'gold'
			}
		},
		addActive(count, level) {
			if (count === Number(level.slice(1,2))) {
				return "active"
			}
		}
	}
}
</script>

<style scoped>
.container {
	display: flex;
}
.synergy {
	display: flex;
	align-items: center;
	margin: 0 2.5px;
	cursor: pointer;
	position: relative;
}
.item-img,
.item-number {
	display: flex;
	align-items: center;
	justify-content: center;
}
.item-img {
	width: 26px;
	height: 30px;
	clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
}
img {
	width: 15.64px;
	height: 15.64px;
}
.item-number {
	height: 15px;
	width: 9px;
	vertical-align: middle;
}
.item-number span {
	color: white;
	font-size: 10px;
	font-weight: 600;
}
.synergy.gold .item-img,
.synergy.gold .item-number {
	background-color: rgb(175, 132, 41);
}
.synergy.silver .item-img,
.synergy.silver .item-number {
	background-color: rgb(113, 113, 113);
}
.synergy.bronze .item-img,
.synergy.bronze .item-number {
	background-color: rgb(101, 66, 33);
}
.synergy.hide {
	display: none;
}
.item-description {
	visibility: hidden;
	position: absolute;
	width: 300px;
	background-color: rgba(0, 0, 0, 0.85);
	padding: 15px 15px;
	top: 54px;
	left: -130px;
	z-index: 1;
	opacity: 0;
	transition: all 0.3s;
	pointer-events: none;
}
.item-description-name {
	color: #3dd0d8;
	font-size: 16px;
	font-weight: 600;
	margin-bottom: 7px;
}
.item-description-title {
	font-size: 14px;
	color: white;
	margin-bottom: 7px;
}
.item-description-level {
	color: #80afdc;
	font-size: 16px;
	font-weight: 600;
}
.item-description-level .active {
	color: #3dd0d8;
}
.synergy:hover .item-description {
	visibility: visible;
	opacity: 1;
}
</style>
