<template>
  <div class="table" oncontextmenu="return false;">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css" />
    <toolbar
      :start="rangeStart"
      :end="rangeEnd"
      :handlerow="handleRow"
      :cell-data="result"
    ></toolbar>
     <table>
        <thead>
          <tr>
            <th
              class="row-col-label"
              v-for="(col, index) in colHead"
              :key="index">
              <span v-if="col.length === 0"></span>
              <col-label v-else
                :col="col"
                :idx="index"
                :asc-sort="ascSort"
                :desc-sort="descSort"
              ></col-label>
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
                :class="{'selected' : cellSelected(rowKey, colKey)}"
            >
              <input v-model="inputIds[rowKey][colKey]">
            </td>
          </tr>
          <tr class="footer">
            <td>Controls</td>
            <td>multiselect cells: enter value into cell then shift + left mouse click</td>
            <td>clear selected cells: right mouse click</td>
            <td colspan="2">calculate total: enter values in cells, 
              select range, choose function from drop down and press enter in input box</td>
          </tr>
        </tbody>
     </table>
     
  </div>
</template>
<script>
import Toolbar from './Toolbar'
import ColLabel from './ColLabel'
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
      row: 3,
      col: 3,
      coors: [],
      grid: [],
      colHead: [''],
      minX: null,
      minY: null,
      maxX: null,
      maxY: null,
      inputIds: [],
      result: []
    }
  },
  methods: {
    resetGrid () {
      for (let i = 0; i <= this.row; i++) {
        for (let j = 0; j <= this.col; j++) {
          if (this.grid[i][j] === 2) {
            this.grid[i].splice(j, 1, 0)
          }
        }
      }
      this.coors = []
      this.result = []
    },
    initInputIds () {
      for (let i = 0; i <= this.row; i++) {
        this.inputIds.push([])
        for (let j = 0; j <= this.col; j++) {
          this.inputIds[i].push('')
        }
      }
    },
    initColHead () {
      this.colHead.push(...'ABCD'.split(''))
    },
    createSpreadSheet () {
      for (let i = 0; i <= this.row; i++) {
        this.grid.push([])
        for (let j = 0; j <= this.col; j++) {
          this.grid[i].push(0)
        }
      }
    },
    selectCell (row, col) {
      this.coors.push({x: row, y: col})
      this.minX = Math.min(...this.coors.map(el => el.x))
      this.maxX = Math.max(...this.coors.map(el => el.x))
      this.minY = Math.min(...this.coors.map(el => el.y))
      this.maxY = Math.max(...this.coors.map(el => el.y))
      this.grid[row].splice(col, 1, 2)
      this.iterateOverGrid(2)
      this.setRange()
    },
    cellSelected (row, col) {
      return (this.grid[row][col] === 2)
    },
    iterateOverGrid (val) {
      for (let i = this.minX; i <= this.maxX; i++) {
        this.grid[i].splice(this.minY, 1, 2)
        for (let j = this.minY; j <= this.maxY; j++) {
          this.grid[i].splice(j, 1, 2)
        }
      }
    },
    setRange () {
      let temp = []
      for (let i = 0; i <= this.row; i++) {
        for (let j = 0; j <= this.col; j++) {
          if (this.grid[i][j] === 2 && this.inputIds[i][j] !== '') {
            let id = this.colHead[1 + j] + (i + 1)
            temp.push({id, value: this.inputIds[i][j]})
          }
        }
      }
      this.result = sortBy(uniqBy(temp, 'id'), 'id')
      return this.result
    },
    handleRow (op) {
      if (op === 1) {
        this.addRow()
      } else if (op === 0) {
        this.removeRow()
      } else {
        return null
      }
    },
    addRow () {
      this.grid.push([])
      this.inputIds.push([])
      for (let j = 0; j < this.col + 1; j++) this.grid[this.grid.length - 1].splice(j, 1, 0)
      for (let j = 0; j < this.col + 1; j++) this.inputIds[this.grid.length - 1].splice(j, 1, '')
    },
    removeRow () {
      this.grid.splice(this.grid.length - 1, 1)
    },
    ascSort (label, index) {
      let temp = []
      for (let i = 0; i < this.row; i++) {
        for (let j = 0; j < index; j++) {
          temp.push(parseInt(this.inputIds[i][j]))
        }
      }
      this.insertSortedArray(temp.sort((a, b) => a - b), index)
    },
    descSort (label, index) {
      let temp = []
      for (let i = 0; i < this.row; i++) {
        for (let j = 0; j < index; j++) {
          temp.push(parseInt(this.inputIds[i][j]))
        }
      }
      // alert(`index: ${index}`)
      this.insertSortedArray(temp.sort((a, b) => b - a), index)
    },
    insertSortedArray (arr, index) {
      for (let i = 0; i < this.row; i++) {
        for (let j = 0; j < index; j++) {
          this.inputIds[i].splice(index - 1, 1, arr[i])
        }
      }
    }
  },
  computed: {
    rangeStart () {
      return this.result.map(el => el.id)[0]
    },
    rangeEnd () {
      return this.result.map(el => el.id)[this.result.length - 1]
    }
  },
  components: {
    Toolbar,
    ColLabel
  }
}
</script>
<style scoped>
  .table{
    margin-top: 45px;
  }
  table {
    width: 75%;
    margin: 0 auto;
  }
  table, th, td {
    border: 2px solid #bbdefb;
    border-collapse: collapse;
  }
  .row-col-label {
    background: #4E7AB5;
    color: #e1f5fe;
  }
  th, td {
    padding: 25px;
  }
  .cell {
    cursor: pointer;
  }
  .selected {
    background: #eeeeee;
    border: 2px solid #64b5f6;
  }
  .clicked {
    border: 5px solid black;
  }
  .footer {
    color: #4E7AB5;
    font-size: 12px;
    text-align: left;
  }
</style>
