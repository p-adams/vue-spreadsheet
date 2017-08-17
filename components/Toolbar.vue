<template>
    <table>
        <thead>
            <tr>
                <th>selected range</th>
                <th>functions</th>
                <th>input</th>
                <th>output</th>
            </tr>
        </thead>
        <tbody>
            <tr class="toolbar">
                <td>
                    <span>
                        <input
                            class="displayRange"
                            :value="startRange"
                            v-model="low"
                            placeholder="A1"
                        />
                        <span class="colon">:</span>
                        <input
                            class="displayRange"
                            :value="endRange"
                            v-model="high"
                            placeholder="A3"
                        />
                    </span>
                </td>
                <td>
                    <div class="selectdiv">
                        <select v-model="selected">
                            <option>SUM</option>
                            <option>AVERAGE</option>
                            <option>MIN</option>
                            <option>MAX</option>
                        </select>
                    </div>
                </td>
                <td>
                    <div>
                        <img
                            src="https://png.icons8.com/sigma/office/30"
                            title="Sigma"
                            width="30"
                            height="30">
                        <span class="equals">
                            =
                        </span>          
                    <input
                        class="result"
                        v-model="input"
                        @change="calculate"
                        placeholder="10+10+10"
                    />
                </div>
                <h6 v-show="missingEquals">input must begin with =</h6>                    
               </td>
               <td>
                   <input
                        class="displayRange"
                        :value="out"
                        placeholder="30">
               </td>
            </tr>
            <tr>
                <td colspan='4'>
                    <button @click="row = 1">add row</button>
                    <button @click="row = 0">remove row</button>
                </td>
            </tr>
        </tbody>
    </table>
</template>
<script>
export default {
  name: 'toolbar',
  props: ['start', 'end', 'handlerow'],
  data () {
    return {
      missingEquals: false,
      low: '',
      high: '',
      input: '',
      out: '',
      row: -1,
      selected: 'SUM'
    }
  },
  watch: {
    low (newVal, oldVal) {
      console.log(`low new ${newVal}`)
    },
    high (newVal, oldVal) {
      console.log(`high new ${newVal}`)
    },
    input (n, o) {
      console.log(n, o)
    },
    row (rowVal, o) {
      this.handlerow(rowVal)
      this.row = -1
    }
  },
  methods: {
    parseExpression (expr) {
      console.log(`parse`)
    },
    calculate () {
      this.out = 30
    }
  },
  computed: {
    startRange () {
      return this.start !== undefined ? this.start : null
    },
    endRange () {
      return this.end !== undefined ? this.end : null
    }
  }
}
</script>
<style scoped>
    table {
        height: 150px;
        background: #eeeeee;
    }
    th {
        color: #4E7AB5;
    }
    td {
        background: white;
        border: 1px solid #9e9e9e;
        padding: 15px;
    }
    input {
        text-indent: 10px;
    }
    .displayRange {
        font-size: 15px;
        border: 1px solid #4E7AB5;
        height: 50px;
        width: 75px;
        border-radius: 2px;
    }
    .selectdiv {
        position: relative;
        float: left;
        min-width: 200px;
    }
    .selectdiv:after {
        content: '\f078';
        font: normal normal normal 17px/1 FontAwesome;
        color: white;
        background: #4E7AB5;
        right: 0;
        top: 6px;
        width: 50px;
        height: 48px;
        padding: 15px 0px 0px 2px;
        border-left: 1px solid white;
        position: absolute;
        pointer-events: none;
    }
    .selectdiv select {
        cursor: pointer;
        -webkit-appearance: none;
        -moz-appearance: none;
        display: block;
        width: 100%;
        max-width: 320px;
        height: 50px;
        float: right;
        margin: 5px 0px;
        padding: 0px 24px;
        font-size: 16px;
        line-height: 1.75;
        border: 1px solid #4E7AB5;
        -ms-word-break: normal;
        word-break: normal;
    }
    .result {
        font-size: 15px;
        border: 1px solid #4E7AB5;
        height: 50px;
        width: 250px;
        border-radius: 2px;
    }
    .equals {
        font-size: 35px;
        color: #4E7AB5;
    }
    button {
        height: 35px;
        width: 150px;
        color: white;
        font-size: 18px;
        line-height: 12px;
        background: #4E7AB5;
        border: none;
        padding: 10px;
        margin: 2px;
        cursor: pointer;
    }
    .colon {
        font-size: 40px;
        color: #4E7AB5;
    }
</style>


