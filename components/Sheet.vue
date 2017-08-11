<template>
  <div>
     <table>
        <thead>
        </thead>
        <tbody>
          {{selected}}
          <tr v-for="(row, index) in rows" :key="index">
            <td v-for="(col, index) in cols" :key="index">
              <div @click="selectCell(row, col)">
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
      rows: [],
      cols: null
    }
  },
  methods:{
    initCols () {
      this.cols = "abcdef"
                    .toUpperCase()
                    .split('')
    },
    initRows () {
      for(let i = 0; i <= 6; i++) {
        this.rows.push(i)
      }
    },
    selectCell (row, col) {
      this.selected = row + col
    }
  }
}
</script>

