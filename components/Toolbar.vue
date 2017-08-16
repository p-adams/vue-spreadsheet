<template>
    <table>
      <tbody>
        <tr>
            <td>
                <input :value="displayRange">
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
                <img src="https://png.icons8.com/sigma/office/30" title="Sigma" width="30" height="30">
            </td>
            <td>
                <input/>
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
</style>


