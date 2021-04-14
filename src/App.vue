<template>
  <div id="app" class="app">
		<div class="row" v-for="(row, i) in field" :key="i">
			<div class="cell" v-for="(cell, j) in row" :key="j" :class="{ active: cell === 1 }"></div>
		</div>
  </div>
</template>
<script>
export default {
	data() {
		return {
			field: [],
			text: ""
		}
	},
	methods: {
		updateState: function() {
		const m = 10
		const n = 10

		const newArray = []
		for (let i = 0; i < this.field.length; i++) {
			newArray.push([...this.field[i]])
		}

		let text = "Новое состояние доски"
		for (let i = 0; i < m; i++) {
			for (let j = 0; j < n; j++) {

					let countForAlive = 0
					let countForDead = 0

					for (let t = Math.max(i - 1, 0); t <= Math.min(i + 1, m - 1); t++) {
						for (let h = Math.max(j - 1, 0); h <=  Math.min(j + 1, n - 1); h++) {
							// counting the number of alive neighbors for an alive cell
							if ((this.field[i][j] === 1) && (this.field[t][h] === 1) && !((t === i) && (h === j))) countForAlive++
							// counting the number of alive neighbors for an dead cell
							if ((this.field[i][j] === 0) && (this.field[t][h] === 1) && !((t === i) && (h === j))) countForDead++
						}
					}
			// rules for changing the state of the cell
			if (countForAlive < 2) newArray[i][j] = 0
      if ((countForAlive === 2) || (countForAlive === 3)) newArray[i][j] = 1
      if (countForAlive > 3) newArray[i][j] = 0
      if (countForDead === 3) newArray[i][j] = 1
			}
		}

		this.field = newArray

		this.text = text 
		}
	},
	created() {},
	mounted() {
		const m = 10
		const n = 10
		for (let i = 0; i < m; i++) {
			const temp = []
			for (let j = 0; j < n; j++) {
		 		temp.push(Math.round(Math.random()))
		 	}
			this.field.push(temp)
		}

		setInterval(() => {
			this.updateState()
		}, 1000)
		
	}
}
</script>
<style lang="less">
.app {
	max-height: 875px;
  height: 50vw;
  margin: 2.5% 10%;
}
	.row {
		display: flex;
		height: 10%;
	}
		.cell {
			width: 10%;
			display: flex;
			border: 1px solid black;
			background: #5E89FE;
		}
			.cell.active {
				background: #D22C00;
			}
</style>
