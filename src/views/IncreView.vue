<template>
  <div>
    <div v-text="num"></div>
    <button @click.stop="incre">증가</button>
 </div>
</template>

<script>
export default {
  name: 'IncreView',
  data () {
    return { num: 1 }
  },
  methods : {
    incre() {
      this.num++;
    }
  }
  
}
</script>

<style>

</style>
