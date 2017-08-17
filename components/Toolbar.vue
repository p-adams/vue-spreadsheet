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
                    <input
                        class="displayRange"
                        :value="displayRange"
                    />
                </td>
                <td>
                    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css" />
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
                        placeholder="=10+10+10"
                    />
                </div>
                <h6 v-show="missingEquals">input must begin with =</h6>                    
               </td>
               <td>
                   <input
                        class="displayRange"
                        placeholder="30">
               </td>
            </tr>
        </tbody>
    </table>
</template>
<script>
export default {
  name: 'toolbar',
  props: ['start', 'end'],
  data () {
    return {
      missingEquals: false,
      range: '',
      input: '',
      out: '',
      selected: 'SUM'
    }
  },
  watch: {
    input (n, o) {
      console.log(n, o)
    }
  },
  computed: {
    displayRange () {
      return this.start !== undefined || this.end !== undefined ? `${this.start}:${this.end}` : null
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
        width: 100px;
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
</style>


