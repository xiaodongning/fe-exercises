<script setup>
import {ref} from 'vue'

const newArray = Array.from(new Set([1,1,2,2,3,4,5,5,6,6,7,8,9,10]))
const printStr = ref(newArray.toString())
</script>

<template>
  <div>
    <highlightjs
     language="js"
     code="const sourceArray = [1,1,2,2,3,4,5,5,6,6,7,8,9,10]
const newArray = Array.from(new Set(sourceArray))
"
    />
    <p>
      newArray result: {{ newArray }}
    </p>
  </div>
</template>


<style scoped>

</style>
