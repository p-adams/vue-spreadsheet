<template>
  <div>
     <table>
        <thead>
          <tr>
            <th v-for="(col, index) in cols" :key="index">
              {{col}}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(row, index) in rows" :key="index">
            <td v-for="(col, index) in cols" :key="index">
              <div
                  :class="{'selected' : cellSelected(row, col)}"
                  @mousedown="selectCell(row, col)">
                <input>
              </div>
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
    this.initCols()
    this.initRows()
  },
  data () {
    return {
      selected: '',
      isSelected: false,
      rows: [],
      cols: null
    }
  },
  methods: {
    initCols () {
      this.cols = 'abcdef'.toUpperCase().split('')
    },
    initRows () {
      for (let i = 1; i <= 6; i++) {
        this.rows.push(i)
      }
    },
    selectCell (row, col) {
      this.selected = row + col
      this.isSelected = true
      this.rows[row] = this.isSelected
      this.cols[col] = this.isSelected
      // alert(this.selected)
    },
    cellSelected (row, col) {
      return (this.rows[row] === this.isSelected && this.cols[col] === this.isSelected)
    }
  }
}
</script>
<style scoped>
  table {
    width: 50%;
  }
  table, th, td {
    border: 1px solid #bdbdbd;
    border-collapse: collapse;
  }
  th {
    background: #00b0ff;
    color: #e1f5fe;
  }
  th, td {
    padding: 5px;
  }
  .selected {
    background: red;
  }
</style>

