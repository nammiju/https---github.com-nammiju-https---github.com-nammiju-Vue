<template>
  <div>
    <h4>{{ title }}</h4>
    <div v-text="cnt"></div>
    <button @click="$emit('enlarge-text')">Enlarge text</button>
    <button @click="$emit('ensmall-text')">Ensmall text</button>
  </div>
</template>

<script>
export default {
    props: {
        title : { type : String, required : true }, 
        cnt : { type : Number, required : false, default : 100}
    },
    data(){
        return {}
    }
}
</script>
