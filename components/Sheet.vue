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
      coors: [],
      grid: [],
      colHead: [' ']
    }
  },
  methods: {
    clicked (row, col) {
      this.grid[row].splice(col, 1, 1)
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
      let start = {x: this.coors[0].x, y: this.coors[0].y}
      let end = {x: this.coors[len].x, y: this.coors[len].y}
      this.grid[row].splice(col, 1, 2)
      for (let i = start.x; i <= end.x; i++) {
        this.grid[i].splice(end.y, 1, 2)
        for (let j = start.y; j <= end.y; j++) {
          this.grid[i].splice(j, 1, 2)
        }
      }
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
    background: gray;
    border: 2px solid #76ff03;
  }
  .clicked {
    border: 5px solid black;
  }
</style>
/* console.log(`i ${i} j ${j}`)
          // console.log(`end.x ${end.x} end.y ${end.y}`)
          // console.log(`startx ${start.x} starty ${start.y}`)
          // if (end.x !== start.x || end.y !== start.y) {
            // this.grid[i].splice(0, i, 2)
            // this.grid[start.x].splice(end.x, 1, 2)
            // console.log(this.grid[i][j])
          // } */
