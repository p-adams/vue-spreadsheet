<template>
  <div>
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
            >{{rowKey+1}}</th>
            <td
                class="cell"
                v-for="(col, colKey, index) in row"
                :key="colKey"
                @click.shift="selectCell(rowKey, colKey)"
                :class="{
                  'selected' : cellSelected(rowKey, colKey)
                  }"
            >
            {{col}}
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
      clicked: false,
      selected: [],
      grid: [],
      colHead: [' '],
      isSelected: false
    }
  },
  methods: {
    clicked (row, col) {
      this.grid[row].splice(col, 1, 1)
    },
    initColHead () {
      this.colHead.push(...'ABC'.split(''))
    },
    createSpreadSheet () {
      for (let i = 0; i <= 2; i++) {
        this.grid.push([])
        for (let j = 0; j <= 2; j++) {
          this.grid[i].push(0)
        }
      }
    },
    selectCell (row, col) {
      this.isSelected = true
      this.grid[row].splice(col, 1, 2)
      this.addSelected(row, col)
    },
    addSelected (row, col) {
      // let column = this.colHead[col + 1]
      // console.log(`col ${column} : ${this.colHead[col + 1]}`)
    },
    cellSelected (row, col) {
      return (this.grid[row][col] === 2)
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
    border: 2px solid #76ff03;
  }
  .clicked {
    border: 5px solid black;
  }
</style>

