<template>
  <div id="topBox">
    <h1>TODO LIST</h1>
    <span class="addBtn" @click="showAddModal">
      <i class="fas fa-plus"></i>
    </span>
    <hr />

    <modal class="modal" v-show="showModal">
      <p slot="header" class="closeBtn"  @click="showAddModal">
        <i class="closeModalBtn fas fa-times" aria-hidden="true"></i>
      </p>
      <p slot="body">
        <DatePicker placeholder="날짜를 선택하세요" :language="ko" format="MM/dd/yyyy" v-model="dateVal" @selected="fixDate"></DatePicker>
        <input type="text" placeholder="할일을 입력하세요" v-model="newItem" @keyup.enter="addTodo"/><br />
      </p>
      <p slot="footer">
        <button class="inputBtn" @click="addTodo">추가</button>
      </p>
    </modal>
  </div>
</template>

<script>
import Modal from "../common/Modal";
import DatePicker from "vuejs-datepicker";
import {en, ko} from 'vuejs-datepicker/dist/locale';
import EventBus from "./EventBus";

export default {
  name: "listTop",
  components: {
    DatePicker,
    Modal
  },
  data(){
    return {
      showModal:false,
      newItem: '',
      newDate: '',
      en: en,
      ko: ko,
      dateVal: ''
    }
  },
  methods: {
    fixDate(date){  // date오브젝트를 MM/dd/yyyy 형식으로 바꿔 newDate에 저장하기
      let year = date.getFullYear();
      let month = (date.getMonth() + 1 < 10) ? `0${date.getMonth() + 1}` : date.getMonth() + 1;
      let day = (date.getDate() < 10 ) ? `0${date.getDate()}` : date.getDate();
      let result = `${month}/${day}/${year}`;
      this.newDate = result;
    },
    showAddModal(){
      this.showModal = !this.showModal;
    },
    addTodo(){
      if(this.newItem !== "" && this.newDate !== ""){
        this.$emit('addStorage', this.newDate, this.newItem);
        this.clearInput();
      }
    },
    clearInput(){
      this.newItem = "";
    },
    inputDate(date){
      this.newDate= date;
    },
    clearDate(){
      this.newDate = '';
    }
  },
  created() {
    EventBus.$on('showAddModal', (date)=>{
      this.newDate = date;
      let year = date.substring(6);
      let month = date.substring(0, 2);
      let day = date.substring(3, 5);
      this.dateVal = new Date(year, month -1, day);
      this.showAddModal();
    });

  }
}
</script>

<style scoped>
h1 {
  font-size: 20px;
  display: inline;
  text-align: left;
}
.addBtn {
  float: right;
  color: red;
}
.closeBtn {
  width:50px;
  text-align: center;
  float: right;
}
.modal input {
  margin-bottom: 10px;
}
hr {
  background-color:black;
  height: 1px;
}
.inputBtn {
}
</style>
