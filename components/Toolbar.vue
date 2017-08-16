<template>
    <table>
        <tbody>
            <tr>
                <td>
                    <input
                        class="displayRange"
                        :value="displayRange"
                        placeholder="A1:A3"
                    />
                </td>
                <td>
                    <select>
                        <option>SUM</option>
                        <option>AVERAGE</option>
                        <option>MIN</option>
                        <option>MAX</option>
                    </select>
                </td>
                <td>
                    <div>
                    <img
                        src="https://png.icons8.com/sigma/office/30"
                        title="Sigma"
                        width="30"
                        height="30">
                    <span>=</span>
                    </div>
                </td>
                <td>
                    <input
                        class="result"
                        placeholder="89.2 + 99.0 + 92.7"
                    />
                </td>
            </tr>
        </tbody>
    </table>
</template>
<script>
export default {
  name: 'toolbar',
  props: ['start', 'end'],
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
    .displayRange {
        font-size: 15px;
        border: 1px solid #e3f2fd;
        height: 30px;
        width: 100px;
        border-radius: 5px;
    }
    .result {
        font-size: 15px;
        border: 1px solid #e3f2fd;
        height: 30px;
        width: 250px;
        border-radius: 5px;
    }
</style>


