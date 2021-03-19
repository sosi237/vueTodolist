<template>
  <div id="bodyBox">
<!-- 등록된 일정이 있으면 날짜별로 보여줌 -->
      <div v-if="todoList && todoList.length > 0">
        <ul>
          <li v-for="(todo, index) in todoList" :key="todo.date"
              @click="showTodo(), showDate(todo.date), setIndex(index)">
            {{todo.date | toDate}}
            <span class="todoCnt">| &nbsp;&nbsp;&nbsp;{{todo.items.length}}</span>
          </li>
        </ul>
      </div>
<!--등록된 일정이 없을 경우-->
      <div class="noContentContainer" v-else>
        <span class="noContent">해야 할 일이 없습니다.</span>
      </div>

<!-- 상세 일정 확인 팝업 모달-->
    <modal v-show="is_show" >
      <p slot="header">
        {{this.curDate | toDate}}
        <span class="closeBtn">
          <i class="closeModalBtn fas fa-times" aria-hidden="true" @click="showTodo"></i>
        </span>
      </p>
      <ul slot="body" v-if="todoList && todoList.length > 0">
        <li v-for="(items, index) in todoList[this.idx].items" :key="items.idx">
          <input type="checkbox" @change="chkTodo(index)" />
          <span v-bind:class="{'completed' : isCompleted(index)}">{{items.item}}</span>
          <span class="removeBtn" @click="removeItem(index)">
            <i class="far fa-trash-alt" aria-hidden="true" ></i>
          </span>
        </li>
      </ul>
      <p slot="footer">
        <span class="addBtn" @click="showAddModal">
          <i class="fas fa-plus"></i>
        </span>
      </p>
    </modal>

  </div>
</template>
<script>
import Modal from "../common/Modal";
import EventBus from "./EventBus";

export default {
  props:['todoList'],
  name: "listBody",
  data(){
    return{
      is_show: false,
      curDate: "",
      idx : 0
    }
  },
  filters:{
    toDate: function (date){
      let year = date.substring(6);
      let month = date.substring(0,2);
      let day = date.substring(3,5);
      let result = `${year}년 ${month}월 ${day}일`;
      return result;
    }
  },
  components: {
    Modal
  },
  methods: {
    chkTodo(detailIdx){
      this.$emit('chkTodo', this.curDate, this.idx, detailIdx);
    },
    isCompleted(detailIdx){ // 해당 세부일정이 완료된 상태인지를 반환
      return this.todoList[ this.idx].items[detailIdx].status;
    },
    showAddModal(){ // 세부목록 모달을 닫고 해당 날짜가 입력된 add모달 열기
      this.is_show = !this.is_show;
      EventBus.$emit('showAddModal', this.curDate);
    },
    showTodo(){
      this.is_show = !this.is_show;
    },
    showDate(date){
      this.curDate = date;
    },
    removeItem(idx){
      this.$emit('removeItem', this.curDate, idx);
    },
    setIndex(index){
      this.idx = index;
    }
  }
}
</script>

<style scoped>
#bodyBox ul {width:80%;}
#bodyBox li {
  border-style: groove; margin-bottom: 15px; padding:5px;
}
.completed {text-decoration: line-through;}
.closeBtn, .removeBtn {float: right;}
ul, li { list-style-type: none;}
.todoCnt {float: right;}
.addBtn {
  float: right;
}
.noContentContainer{
  text-align: center;
}
</style>
