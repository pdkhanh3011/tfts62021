<template>
	<div class="builder">
		<div
			v-for="n in 28"
			:key="n"
			class="builder-item"
			:data="n"
			:class="addBorder(n)"
			:draggable="dragAble(n)"
			@dragover.prevent="onDragOver(n)"
			@drop="onDrop($event, n)"
			@dragleave="onDragLeave(n)"
			@dragstart="onDragStart($event,n)"
			@dragend="onDragEnd()"
		>
			<div class="builder-item-contaier">
				<div class="builder-item-top">
					<div class="builder-top-img">
						<img :src="addImgChampion(n)" alt="" />
					</div>
				</div>
				<div class="builder-item-bottom">
					<div class="builder-bottom-img">
						<img :src="addImgChampion(n)" alt="" />
					</div>
				</div>
				<div class="builder-middle-img">
					<img :src="addImgChampion(n)" alt="" />
				</div>
				<div v-if="slotChampion(n)" class="builder-synergy">
					<div
						v-for="(item, index1) in addSynergy(n)"
						:key="index1"
						class="builder-synergy-item"
					>
						<img :src="addImgSynergy(item)" alt="" />
					</div>
				</div>
				<div v-if="slotChampion(n)" class="builder-name">
					<span>{{ addName(n) }}</span>
				</div>
				<div v-if="slotChampion(n)" class="builder-equiment">
					<div
						v-for="(img, index) in addImgsItem(n)"
						:key="index"
						class="builder-equiment-item"
						draggable="true"
						@dragstart.stop=""
					>
						<img :src="img" alt="" />
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	props: {
		championsInBoard: {
			type: Array,
			default: null,
		},
		championData: {
			type: Array,
			default: null,
		},
		synergiesData: {
			type: Array,
			default: null,
		},
		itemsData: {
			type: Array,
			default: null,
		},
	},
	methods: {
		addBorder(n) {
			if (this.championsInBoard) {
				const check = this.championsInBoard.find(
					(item) => item.position === n
				)
				if (check) {
					const price = this.championData.find(
						(item) => item.name === check.name
					).price
					return `borders-${price}`
				}
			}
		},
		addImgChampion(n) {
			if (this.championsInBoard) {
				const check = this.championsInBoard.find(
					(item) => item.position === n
				)
				if (check) {
					const img = this.championData.find(
						(item) => item.name === check.name
					).img_hero
					return img
				}
			}
		},
		addName(n) {
			if (this.championsInBoard) {
				const check = this.championsInBoard.find(
					(item) => item.position === n
				)
				if (check) {
					const name = this.championData.find(
						(item) => item.name === check.name
					).name
					return name
				}
			}
		},
		addSynergy(n) {
			if (this.championsInBoard) {
				const check = this.championsInBoard.find(
					(item) => item.position === n
				)
				if (check) {
					const toc = this.championData.find(
						(item) => item.name === check.name
					).toc
					const he = this.championData.find(
						(item) => item.name === check.name
					).he
					if (he!=='') {
						return (toc + ',' + he).split(',')
					} else {
						return toc.split(',')
					}
				}
			}
		},
		slotChampion(n) {
			if (this.championsInBoard) {
				return this.championsInBoard.find((item) => item.position === n)
			}
		},
		addImgSynergy(synergyName) {
			return this.synergiesData.find((item) => item.name === synergyName)
				.img_link
		},
		addImgsItem(n) {
			if (this.championsInBoard) {
				const check = this.championsInBoard.find(
					(item) => item.position === n
				)
				if (check && check.items != null) {
					const arr = check.items
					// eslint-disable-next-line array-callback-return
					const arrImgLink = arr.map((item) => {
						if (
							this.itemsData.find((item1) => item1.name === item)
						) {
							return this.itemsData.find(
								(item1) => item1.name === item
							).img_item
						}
					})
					return arrImgLink
				}
			}
		},
		onDrop(e, n) {
			const name = e.dataTransfer.getData('name')
			if (name) {
				this.$emit('on-drop', {
					name,
					position: n,
					items: [],
				})
			}
			const nameInBoard = e.dataTransfer.getData('nameInBoard')
			const positionInBoard = Number(e.dataTransfer.getData('positionInBoard'))
			if (nameInBoard && positionInBoard) {
				this.$emit('move-champion', {
					name: nameInBoard,
					oldPosition: positionInBoard,
					newPosition: n
				})
			}
			const nameItem = e.dataTransfer.getData('nameItem')
			if (nameItem) {
				this.$emit('add-item-to-champion', {
					nameItem,
					positionOfChampion: n
				})
			}
			const builderItems = document.querySelectorAll('.builder-item')
			builderItems[n - 1].classList.remove('on-drag-over')
		},
		onDragOver(n) {
			const builderItems = document.querySelectorAll('.builder-item')
			builderItems[n - 1].classList.add('on-drag-over')
		},
		onDragLeave(n) {
			const builderItems = document.querySelectorAll('.builder-item')
			builderItems[n - 1].classList.remove('on-drag-over')
		},
		onDragStart(e,n) {
			if (this.championsInBoard) {
				const check = this.championsInBoard.find(
					(item) => item.position === n
				)
				if (check) {
					e.dataTransfer.setData('nameInBoard', check.name)
					e.dataTransfer.setData('positionInBoard', check.position)
				}
				const builderitem = document.querySelectorAll('.builder-item')
				builderitem.forEach(element => {
					element.classList.add('on-start-drag')
				});
				const list = document.querySelector('.list')
				list.classList.add('drop')
			}
		},
		dragAble(n) {
			if (this.championsInBoard) {
				const check = this.championsInBoard.find(
					(item) => item.position === n
				)
				if (check) {
					return true
				} else {
					return false
				}
			} else {
				return false
			}
		},
		onDragEnd() {
			const builderitem = document.querySelectorAll('.builder-item')
            builderitem.forEach(element => {
                element.classList.remove('on-start-drag')
            });
			const list = document.querySelector('.list')
			list.classList.remove('drop')
		}
	},
}
</script>

<style scoped>
.builder {
	display: flex;
	flex-wrap: wrap;
	margin-bottom: 50px;
}
.builder-item {
	position: relative;
	border-left: 4px solid;
	border-right: 4px solid;
	width: 84px;
	height: 48.5px;
	margin: 24.25px 4px 25.25px;
}
.builder-item-container {
	display: block;
}
.builder-item:nth-child(8) {
	margin-left: 44px;
}
.builder-item:nth-child(22) {
	margin-left: 44px;
}
.builder-item-top {
	border-top: 5.6569px solid;
	border-right: 5.6569px solid;
	position: absolute;
	width: 59.4px;
	height: 59.4px;
	transform: scaleY(0.5774) rotate(-45deg);
	top: -30px;
	left: 8px;
	overflow: hidden;
	z-index: 1;
}
.builder-item-bottom {
	position: absolute;
	border-left: 5.6569px solid;
	border-bottom: 5.6569px solid;
	width: 59.4px;
	transform: scaleY(0.5774) rotate(-45deg);
	height: 59.4px;
	top: 19px;
	left: 9px;
	overflow: hidden;
	z-index: 1;
}
.builder-item-img {
	position: absolute;
	width: 76px;
	transform: rotate(45deg) scaleY(1.7321) translateY(-21.9393px);
}
.builder-item-top > img {
	position: absolute;
	width: 100%;
	height: auto;
}
.builder-item-bottom > img {
	position: absolute;
	transform: rotate(45deg) scaleY(1.7321) translateY(-21.9393px);
	width: 76px !important;
	height: auto;
}
.builder-middle-img {
	position: absolute;
	overflow: hidden;
	height: 48.5px;
	top: 1px;
}
.builder-middle-img > img {
	width: 100%;
	height: auto;
	vertical-align: middle;
	transform: translateY(-20%) scale(1.17);
}
.builder-top-img {
	position: absolute;
	transform: rotate(45deg) scaleY(1.7321);
	width: 76px;
	top: 16px;
	left: -38px;
}
.builder-top-img img {
	width: 100%;
	transform: scale(1.17);
}
.builder-bottom-img {
	position: absolute;
	transform: rotate(45deg) scaleY(1.7321);
	width: 76px;
	top: -39px;
	left: 16px;
}
.builder-bottom-img img {
	width: 100%;
	transform: scale(1.17);
}
.builder-item.borders-1,
.builder-item.borders-1 .builder-item-top,
.builder-item.borders-1 .builder-item-bottom {
	border-color: gray;
}
.builder-item.borders-2,
.builder-item.borders-2 .builder-item-top,
.builder-item.borders-2 .builder-item-bottom {
	border-color: #11b288;
}
.builder-item.borders-3,
.builder-item.borders-3 .builder-item-top,
.builder-item.borders-3 .builder-item-bottom {
	border-color: #207ac7;
}
.builder-item.borders-4,
.builder-item.borders-4 .builder-item-top,
.builder-item.borders-4 .builder-item-bottom {
	border-color: #c440da;
}
.builder-item.borders-5,
.builder-item.borders-5 .builder-item-top,
.builder-item.borders-5 .builder-item-bottom {
	border-color: #ffb93b;
}
.builder-synergy {
	position: absolute;
	display: flex;
	justify-content: center;
	top: -25px;
	left: 50%;
	transform: translateX(-50%);
	z-index: 9;
}
.builder-synergy-item {
	width: 24px;
	height: 24px;
	background-image: url(https://lolchess.gg/images/tft/traits/background/black.svg);
	background-size: cover;
	display: flex;
	justify-content: center;
	align-items: center;
}
.builder-synergy-item > img {
	width: 14px;
	height: 14px;
	pointer-events: none;
}
.builder-name {
	position: absolute;
	left: 50%;
	transform: translateX(-50%);
	z-index: 9;
	top: 25px;
}
.builder-name > span {
	text-shadow: -1px 0 2px #000, 0 1px 2px #000, 1px 0 2px #000,
		0 -1px 2px #000;
	font-size: 11px;
	font-weight: 400;
	color: white;
}
.builder-equiment {
	position: absolute;
	left: 50%;
	transform: translateX(-50%);
	z-index: 9;
	display: flex;
	top: 50px;
}
.builder-equiment-item {
	width: 24px;
	height: 24px;
}
.builder-equiment-item > img {
	width: 100%;
	height: auto;
	pointer-events: none;
}
.builder-item-top img,
.builder-item-bottom img,
.builder-middle-img img {
	pointer-events: none;
}
.draghehe {
	width: 100%;
	height: auto;
}
.builder-item.on-start-drag,
.builder-item.on-start-drag .builder-item-top,
.builder-item.on-start-drag .builder-item-bottom {
	background-color: rgb(56, 56, 56);
}
.builder-item.on-drag-over,
.builder-item.on-drag-over .builder-item-top,
.builder-item.on-drag-over .builder-item-bottom {
	background-color: rgb(0, 87, 114);
}
</style>
