<template>
  <div body oncontextmenu="return false;">
    {{rangeStart}}:{{rangeEnd}}
    <select>
      <option>SUM</option>
      <option>AVERAGE</option>
      <option>MIN</option>
      <option>MAX</option>
    </select>
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
                @click.shift="selectCell(rowKey, colKey)"
                :class="{
                    'selected' : cellSelected(rowKey, colKey)
                    }"
            >
              {{rowKey}} {{colKey}}
              <input v-model="inputIds[rowKey][colKey]">
            </td>
          </tr>
        </tbody>
     </table>
  </div>
</template>
<script>
import uniqBy from 'lodash/uniqBy'
import sortBy from 'lodash/sortBy'
export default {
  name: 'sheet',
  created () {
    this.initColHead()
    this.createSpreadSheet()
    this.initInputIds()
  },
  data () {
    return {
      selected: false,
      disableCursor: false,
      size: 3,
      coors: [],
      grid: [],
      colHead: [' '],
      start: '',
      end: '',
      inputIds: [],
      result: []
    }
  },
  methods: {
    resetGrid () {
      for (let i = 0; i <= this.size; i++) {
        for (let j = 0; j <= this.size; j++) {
          if (this.grid[i][j] === 2) {
            this.grid[i].splice(j, 1, 0)
          }
        }
      }
      this.coors = []
      this.start = ''
      this.end = ''
    },
    initInputIds () {
      for (let i = 0; i <= this.size; i++) {
        this.inputIds.push([])
        for (let j = 0; j <= this.size; j++) {
          this.inputIds[i].push('')
        }
      }
    },
    initColHead () {
      this.colHead.push(...'ABCD'.split(''))
    },
    createSpreadSheet () {
      for (let i = 0; i <= this.size; i++) {
        this.grid.push([])
        for (let j = 0; j <= this.size; j++) {
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
      this.setRange()
    },
    cellSelected (row, col) {
      return (this.grid[row][col] === 2)
    },
    iterateOverGrid (col) {
      // the starting y position is less than or equal to ending y position
      if (this.start.y <= this.end.y) {
        for (let i = this.start.x; i <= this.end.x; i++) {
          for (let j = this.start.y; j <= this.end.y; j++) {
            this.grid[i].splice(j, 1, col)
          }
        }
      } else {
        // the starting y position is greater than the ending y position
        for (let i = this.start.y; i >= this.end.y; i--) {
          this.grid[this.start.x].splice(i, 1, 2)
          for (let j = this.end.x; j >= this.start.x; j--) {
            this.grid[j].splice(i, 1, col)
          }
        }
      }
    },
    setRange () {
      let temp = []
      for (let i = 0; i <= this.size; i++) {
        for (let j = 0; j <= this.size; j++) {
          if (this.grid[i][j] === 2 && this.inputIds[i][j] !== '') {
            let id = this.colHead[1 + j] + (i + 1)
            temp.push({id, value: this.inputIds[i][j]})
          }
        }
      }
      this.result = sortBy(uniqBy(temp, 'id'), 'id')
      return this.result
    }
  },
  computed: {
    rangeStart () {
      return this.result.map(el => el.id)[0]
    },
    rangeEnd () {
      return this.result.map(el => el.id)[this.result.length - 1]
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
