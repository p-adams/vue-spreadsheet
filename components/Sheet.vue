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
                v-for="(col, colKey, index) in row"
                :key="colKey"
                :class="{'selected' : cellSelected(rowKey, colKey)}"
                @click="selectCell(rowKey, colKey)"
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
      selected: '',
      grid: [],
      colHead: [' '],
      isSelected: false
    }
  },
  methods: {
    initColHead () {
      this.colHead.push(...'ABCDEF'.split(''))
    },
    createSpreadSheet () {
      for (let i = 0; i <= 5; i++) {
        this.grid[i] = []
        for (let j = 0; j <= 5; j++) {
          this.grid[i][j] = false
        }
      }
    },
    selectCell (row, col) {
      console.log(`row ${row} col ${col}`)
      this.selected = row + col
      this.isSelected = true
    },
    cellSelected (row, col) {
      return (this.grid[row][col] === true)
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
    padding: 15px;
  }
  .selected {
    background: red;
  }
  .foo {
    height: 50px;
    width: 50px;
  }
</style>

