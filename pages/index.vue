<template>
	<div class="wrapper">
		<div class="container">
			<div
				v-for="(team, teamname) in metalist"
				:key="teamname"
				class="item"
			>
				<div class="name">
					<span>{{ teamname }}</span>
				</div>
				<div class="team">
					<avatar
						v-for="(champion, championname) in team"
						:key="championname"
						class="member"
						:width="38"
						:height="38"
						:level="champion.level"
						:item-list="getImgItem(champion.items)"
						:champion-info="getChampionInfo(championname)"
						:synergy-data="synergyData"
					></avatar>
				</div>
				<div class="passive">
					<synergylist
						:synergy-list="getSynergyList(team)"
					></synergylist>
				</div>
				<div class="detail">
					<button class="detail-btn">Chi Tiết</button>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import avatar from '~/components/tuong/avatar.vue'
import synergylist from '~/components/synergy/synergylist.vue'
export default {
	components: {
		avatar,
		synergylist,
	},
	async asyncData() {
		const response1 = await fetch(
			'https://tftbuild-bcbec-default-rtdb.asia-southeast1.firebasedatabase.app/2/data.json'
		)
		const championData = await response1.json()
		const response2 = await fetch(
			'https://tftbuild-bcbec-default-rtdb.asia-southeast1.firebasedatabase.app/3/data.json'
		)
		const itemData = await response2.json()
		const response3 = await fetch(
			'https://tftbuild-bcbec-default-rtdb.asia-southeast1.firebasedatabase.app/4/data.json'
		)
		const synergyData = await response3.json()
		return { championData, itemData, synergyData }
	},
	data() {
		return {
			metalist: {
				'Oriana Seraphine carry': {
					jayce: { level: 2, items: ['bramble', 'claw', 'titan'] },
					orianna: { level: 2, items: ['shojin', 'staff', 'shojin'] },
					seraphine: {
						level: 2,
						items: ['shojin', 'staff', 'shojin'],
					},
					braum: { level: 2, items: ['sunfire'] },
					leona: { level: 2, items: [] },
					taric: { level: 2, items: [] },
					janna: { level: 2, items: [] },
					yuumi: { level: 2, items: [] },
				},
				"Kog'maw Hộ vệ": {
					garen: { level: 3, items: ['warmog', 'titan', 'claw'] },
					Kassadin: { level: 3, items: [] },
					caitlyn: { level: 3, items: [] },
					kogmaw: {
						level: 2,
						items: ['justice', 'rageblade', 'hurricane'],
					},
					sion: { level: 2, items: [] },
					chogath: { level: 2, items: [] },
				},
			},
		}
	},
	methods: {
		getImgItem(itemsArr) {
			const itemList = itemsArr.map((item) => {
				return this.itemData.find((itemInItemdata) =>
					itemInItemdata.name.toLowerCase().includes(item)
				).img_item
			})
			return itemList
		},
		getChampionInfo(championName) {
			return this.championData.find(
				(item) => item.name.toLowerCase() === championName.toLowerCase()
			)
		},
		getSynergyList(team) {
			const data = []
			for (const champion in team) {
				data.push(
					this.championData.find(
						(item) =>
							item.name.toLowerCase() === champion.toLowerCase()
					).toc
				)
				data.push(
					this.championData.find(
						(item) =>
							item.name.toLowerCase() === champion.toLowerCase()
					).he
				)
			}
			const arr = data.join(',').split(',')
			function unique(arr) {
				return Array.from(new Set(arr))
			}
			const objectData = []
			const arrUnique = unique(arr)
			arrUnique.forEach((element) => {
				let count = 0
				for (const item in arr) {
					if (element === arr[item]) {
						count = count + 1
					}
				}
				const data = this.synergyData.filter(
					(item) => item.name === element
				)

				objectData.push({
					count,
					...data,
				})
			})
			return objectData
		},
	},
}
</script>

<style scoped>
.wrapper {
	background-color: #1e1d32;
	padding: 50px 0;
	height: auto;
}
.container {
	max-width: 1080px;
	width: 100%;
	margin: 0 auto;
}
.item {
	display: flex;
	margin: 18px 0;
	width: 100%;
	border: 1px solid rgb(201, 89, 89);
	background-color: #292845;
	align-items: center;
	height: 95px;
	padding: 0 30px;
}
.name {
	color: white;
	width: 160px;
	font-weight: 700;
}
.team {
	display: flex;
	width: 435px;
}
.member {
	margin: 0 4px;
}
.detail {
	margin-left: auto;
}
.detail-btn {
	display: block;
	width: 120px;
	height: 43px;
	color: white;
	border-radius: 5px;
	font-weight: 600;
	background-color: rgb(122, 102, 224);
	font-size: 16px;
	padding-bottom: 2px;
}
</style>
