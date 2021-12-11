<template>
	<div class="wrapper">
		<div class="container">
			<div class="synergy-container">
				<BuilderSynergylistSynergy-item
					:synergies-sort-by-des="synergiesSortByDes"
					:synergies-data="synergiesData"
				/>
			</div>
			<div class="builder-container">
				<div class="builder-main">
					<builderBoard
						:champions-in-board="championsInBoard"
						:champion-data="championData"
						:synergies-data="synergiesData"
						:items-data="itemsData"
						@on-drop="onDrop($event)"
						@move-champion="moveChampion($event)"
						@add-item-to-champion="addItemToChampion($event)"
					/>
				</div>
				<div class="list-items">
					<BuilderItemListitems 
					:items-data="itemsData"/>
				</div>
				<div class="list-champion-container">
					<BuilderChampionListchampion
						:champion-data="championData"
						@remove-champion="removeChampion($event)"
					/>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import builderBoard from '~/components/builder/board/board.vue'
export default {
	components: { builderBoard },
	async asyncData() {
		const res = await fetch('http://phplaravel-701092-2318612.cloudwaysapps.com/synergies')
		const synergiesData = await res.json()

		const res2 = await fetch('http://phplaravel-701092-2318612.cloudwaysapps.com/champions')
		const championData = await res2.json()

		const res3 = await fetch('http://phplaravel-701092-2318612.cloudwaysapps.com/items')
		const itemsData = await res3.json()
		return { synergiesData, championData, itemsData }
	},
	data() {
		return {
			championsInBoard: [
				
			],
		}
	},
	computed: {
		synergiesSortByDes() {
			if (this.synergies) {
				// eslint-disable-next-line vue/no-side-effects-in-computed-properties
				return this.synergies.sort((a, b) => {
					if (a.level === b.level) {
						return a.count - b.count
					} 
					
					return b.level - a.level
				})
			} else {
				return null
			}
		},
		synergies() {
			if (this.championsInBoard.length !== 0) {
				const championList = this.championsInBoard.map((e) => e.name)
				const championItems = []
				this.championsInBoard.forEach(function(e) {
					championItems.push(...e.items)
				})
				const championSynergyItems = championItems.map(e => {
					return this.itemsData.find(a => a.name === e).type
				})
				const championListUnique = Array.from(new Set(championList))
				const data = championListUnique.map((name) => {
					const toc = this.championData.find(
						(a) => a.name === name
					).toc
					
					const he = this.championData.find((a) => a.name === name).he
					if (he!==''&toc!==''){
						return toc + ',' + he
						
					} else if (he==='') {
						return toc
					} else {
						return he
					}
					
				})
				const arrSynergies = data.join(',').split(',')
				const arrSynergiesUnique = Array.from(new Set(arrSynergies))
				const synergies = []
				arrSynergiesUnique.forEach((element) => {
					let count = 0
					for (const item in arrSynergies) {
						if (element === arrSynergies[item]) {
							count = count + 1
						}
					}
					for (const itemSynergy in championSynergyItems) {
						if (element === championSynergyItems[itemSynergy]) {
							count = count + 1
						}
					}
					let level = 0
					const statesArr = this.synergiesData
						.find((item) => item.name === element)
						.state.split('hehe')
					// eslint-disable-next-line eqeqeq
					if (statesArr.length === 1 && statesArr[0] == 1) {
						level = 3
						// eslint-disable-next-line eqeqeq
					} else if (statesArr.length === 1 && statesArr[0] == 2) {
						for (const state in statesArr) {
							if (count >= statesArr[statesArr.length - 1]) {
								level = 3
							} else if (count >= statesArr[state]) {
								level += 1
							}
						}
					} else if (statesArr.length === 2) {
						for (const state in statesArr) {
							if (count >= statesArr[statesArr.length - 1]) {
								level = 3
							} else if (count >= statesArr[state]) {
								level += 1
							}
						}
					} else if (statesArr.length === 3) {
						for (const state in statesArr) {
							if (count >= statesArr[statesArr.length - 1]) {
								level = 3
							} else if (count >= statesArr[1]) {
								level = 3
							} else if (count >= statesArr[state]) {
								level += 1
							}
						}
					} else {
						for (const state in statesArr) {
							if (count >= statesArr[state]) {
								level += 1
							}
						}
					}
					synergies.push({
						name: element,
						count,
						level,
					})
				})
				return synergies
			} else {
				return null
			}
		},
	},
	methods: {
		onDrop(obj) {
			const index = this.championsInBoard.findIndex(item => item.position === obj.position)
			if (index >= 0) {
				this.championsInBoard.splice(index, 1)
				this.championsInBoard.push(obj)
			} else {
				this.championsInBoard.push(obj)
			}
		},
		moveChampion(event) {
			if (this.championsInBoard.length !== 0) {
			this.championsInBoard.forEach((item,index)=> {
				if (event.name === item.name && event.oldPosition === item.position){
					if (this.championsInBoard) {
						const check = this.championsInBoard.find(
							(item) => item.position === event.newPosition
						)
						if (check) {
							const i = this.championsInBoard.findIndex(e=>e.position === event.newPosition)
							this.championsInBoard[index].position = event.newPosition
							this.championsInBoard[i].position = event.oldPosition
						} else {
							this.championsInBoard[index].position = event.newPosition
						}
					}
				}
			})
			}
		},
		removeChampion(e) {
			if (this.championsInBoard.length !== 0) {
				const index = this.championsInBoard.findIndex(item=>{
					return item.name === e.nameInBoard && item.position === Number(e.positionInBoard)
				})
				if (index >= 0) {
					this.championsInBoard.splice(index,1)
				}
			}
		},
		addItemToChampion(e) {
			if (this.championsInBoard.length !== 0) {
				const index = this.championsInBoard.findIndex(item=>{
					return item.position === Number(e.positionOfChampion)
				})
				if(index >= 0) {
					if (this.championsInBoard[index].items.length<=2) {
						this.championsInBoard[index].items.push(e.nameItem)
					}
				}
			}
		}
	},
}
</script>

<style scoped>
.wrapper {
	padding-top: 50px;
	width: 100%;
	background-color: #212529;
	height: auto;
	user-select: none;
}
.container {
	max-width: 1140px;
	width: 100%;
	height: auto;
	margin: 0 auto;
	display: flex;
}
.synergy-container {
	padding: 0 8px;
	width: 16.6666666667%;
	z-index: 10;
}
.builder-container {
	max-width: 83.3333333333%;
	width: 100%;
	display: flex;
	flex-wrap: wrap;
	height: min-content;
}
.list-champion-container {
	width: 75%;
}
.builder-main {
	width: 75%;
}
.list-items {
	width: 25%;
}
</style>
