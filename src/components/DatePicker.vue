<template>
  <input v-model="date" type="text" name="date"
    id="calendar" placeholder="날짜를 선택해주세요" >

</template>

<script>
export default {
  name: "DatePicker",
  props: ['newDate'],
  data(){
    return {
      date: ""
    }
  },
  methods:{
  },
  watch:{
    newDate(){
      this.date = this.newDate;
    }
  },
  mounted() {
    let vm = this
    $('#calendar').datepicker({
      onSelect: function (dateText){
        vm.date = dateText
      }
    });
  },
  updated() {
    this.$emit('inputDate', this.date);
  }
}
</script>

<style scoped>

</style>
