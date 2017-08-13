<template>
  <div body oncontextmenu="return false;">
     <table>
        <thead>
          <tr>
            <th
              class="row-col-label"
              v-for="(col, index) in colHead"
              :key="index">
              {{col}}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(row, rowKey, index) in grid"
            :key="rowKey">
            <th
              class="row-col-label"
            >
              {{rowKey + 1}}
            </th>
            <td
                class="cell"
                v-for="(col, colKey, index) in row"
                :key="colKey"
                @contextmenu="resetGrid"
                @mouseover.shift="selectCell(rowKey, colKey)"
                :class="{'selected' : cellSelected(rowKey, colKey)}"
            >
              {{grid[rowKey][colKey]}}
              <input v-model="grid[rowKey][colKey]">
            </td>
          </tr>
        </tbody>
     </table>
  </div>
</template>
<script>
export default {
  name: 'sheet',
  created () {
    this.initColHead()
    this.createSpreadSheet()
  },
  data () {
    return {
      selected: false,
      coors: [],
      grid: [],
      colHead: [' '],
      start: '',
      end: ''
    }
  },
  methods: {
    resetGrid () {
      this.iterateOverGrid(0)
    },
    initColHead () {
      this.colHead.push(...'ABCD'.split(''))
    },
    createSpreadSheet () {
      for (let i = 0; i <= 3; i++) {
        this.grid.push([])
        for (let j = 0; j <= 3; j++) {
          this.grid[i].push(0)
        }
      }
    },
    selectCell (row, col) {
      this.coors.push({x: row, y: col})
      let len = this.coors.length - 1
      this.start = {x: this.coors[0].x, y: this.coors[0].y}
      this.end = {x: this.coors[len].x, y: this.coors[len].y}
      this.grid[row].splice(col, 1, 2)
      this.iterateOverGrid(2)
    },
    cellSelected (row, col) {
      return (this.grid[row][col] === 2)
    },
    showCoors (row, col) {
      return `${row} ${col}`
    },
    iterateOverGrid (col) {
      for (let i = this.start.x; i <= this.end.x; i++) {
        this.grid[i].splice(this.end.y, 1, col)
        for (let j = this.start.y; j <= this.end.y; j++) {
          this.grid[i].splice(j, 1, col)
        }
      }
    }
  }
}
</script>
<style scoped>
  table {
    width: 100%;
  }
  table, th, td {
    border: 1px solid #bdbdbd;
    border-collapse: collapse;
  }
  .row-col-label {
    background: #00b0ff;
    color: #e1f5fe;
  }
  th, td {
    padding: 25px;
  }
  .cell {
    cursor: pointer;
  }
  .selected {
    background: #bdbdbd;
    border: 2px solid #76ff03;
  }
  .clicked {
    border: 5px solid black;
  }
</style>
