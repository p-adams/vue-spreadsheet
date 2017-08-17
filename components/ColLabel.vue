<template>
    <div>
        <i class="fa fa-arrow-up" aria-hidden="true"></i> <span>{{col}}</span>
        <i class="fa fa-arrow-down" aria-hidden="true"></i>
    </div>
</template>
<script>
export default {
  name: 'label',
  props: ['col']
}
</script>
<style scoped>
    .fa.fa-arrow-up {
        cursor: pointer;
        padding: 10px;
    }
    .fa.fa-arrow-down {
        cursor: pointer;
         padding: 10px;
    }
</style>

