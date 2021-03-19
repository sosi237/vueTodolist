<template>
  <div id="topBox">
    <h1>TODO LIST</h1>
    <span class="addBtn" @click="showAddModal">
      <i class="fas fa-plus"></i>
    </span>
    <hr />

    <modal class="modal" v-show="showModal">
      <p slot="header" class="closeBtn">
        <i class="closeModalBtn fas fa-times " aria-hidden="true" @click="showAddModal(), clearDate()"></i>
      </p>
      <p slot="body">
        <DatePicker v-on:inputDate="inputDate" v-bind:newDate="newDate"></DatePicker>
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
import DatePicker from "../DatePicker";
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
      newDate: ''
    }
  },
  methods: {
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
