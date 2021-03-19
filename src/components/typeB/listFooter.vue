<template>
  <div class="clearBtnContainer">
    <span class="clearBtn" @click="$emit('clearAll')">clear all</span>
  </div>
</template>

<script>
export default {
  name: "listFooter"
}
</script>

<style scoped>
.clearBtnContainer {
  width:5.5rem;
  height:35px;
  line-height: 30px;
  background-color: lightblue;
  border-radius: 5px;
  text-align: center;
  margin:30px auto;
}
.clearBtn {
  color: black;
  display: block;

}
</style>
