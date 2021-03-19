<template>
  <div id="app">
    <list-top @addStorage="addStorage"></list-top>
    <list-body v-bind:todoList="todoList" @removeItem="removeItem" @chkTodo="chkTodo"></list-body>
    <list-footer @clearAll="clearAll"></list-footer>
  </div>
</template>

<script>
import ListTop from "./components/typeB/listTop";
import ListBody from "./components/typeB/listBody";
import ListFooter from "./components/typeB/listFooter";

export default {
  name: 'app',
  components: {ListFooter, ListBody, ListTop},
  data () {
    return {
      todoList: []
    }
  },
  methods: {
    addList(){  //로컬 스토리지에 저장된 일정들을 todoList 데이터에 저장
      this.todoList = [];
      if(localStorage.length > 0) {
        for (let i = 0; i < localStorage.length; i++) {
          let key = localStorage.key(i);
          if (key != "loglevel:webpack-dev-server") {
            let items = []; // 특정 날짜의 모든 할일 object를 담을 배열
            let arrValues = JSON.parse(localStorage.getItem(key));  // 특정 날짜에 들어있는 할일들 담을 배열
            for(let j = 0; j < arrValues.length; j++) {
              let item = { // 각 할일의 인덱스, 일정내용, 완료상태를 담을 object
                'idx': j,
                "item" : arrValues[j],
                "status" : false,
              };
              items.push(item);
            }
            let obj = {
              'date': key,
              'items': items
            }
            this.todoList.push(obj);
          }
        }
        this.todoList.sort(function (a, b){   // 리스트에서 날짜별로 오름차순 정렬 위해 sort()
          return a.date < b.date ? -1 : (a.date > b.date ? 1 : 0);
        });
      }
    },
    addStorage(newDate, newItem){ // 입력한 일정을 로컬 스토리지에 저장
      let items = [];
      if(this.chkNew(newDate)){  // 해당 날짜에 일정을 등록하는 게 처음이면
        items.push(newItem.trim());
        localStorage.setItem(newDate, JSON.stringify(items));
      } else { // 기존 날짜에 일정을 추가하면
        items = JSON.parse(localStorage.getItem(newDate));
        items.push(newItem.trim());
        localStorage.setItem(newDate, JSON.stringify(items));
      }
      this.addList();
    },
    chkNew(date){ // 입력한 날짜에 해당하는 일정이 이미 등록되어 있는지 확인하는 메소드(처음이면 true, 아니면 false )
      for(let i = 0; i < localStorage.length; i++){
        if (localStorage.key(i) == date)  return false;
      }
      return true;
    },
    removeItem(date, index){
      let arrVal = JSON.parse(localStorage.getItem(date)); //로컬 스토리지에 들어있는 세부일정 배열 받아옴
      if(arrVal.length > 1){
        arrVal.splice(index, 1);
        localStorage.setItem(date, JSON.stringify(arrVal));
        this.addList();
      } else {
        localStorage.removeItem(date);
        // this.addList();
      }
    },
    chkTodo(date, listIdx, detailIdx){
      this.todoList[listIdx].items[detailIdx].status = !this.todoList[listIdx].items[detailIdx].status;
    },
    clearAll(){
      localStorage.clear();
      this.todoList = [];
    }
  },
  created() {
    this.addList();
  }
}
</script>

<style>
</style>
