<template>
  <div class="row mb-3">
    <div class="col">
      <div class="input-group">
        <input
          type="text"
          class="form-control"
          placeholder="할일 입력창"
          v-model.trim="todo"
          @keyup.enter="addTodoHandler"
        />
        <div>
          마감일 :
          <input
            type="date"
            class="form-control"
            placeholder="마감일"
            v-model="duedate"
          />
        </div>

        <button class="btn btn-primary" type="button" @click="addTodoHandler">
          추가
        </button>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref } from 'vue';
//내보낼때 todo:'' (객체형태)로 전송됨
const todo = ref('');
const duedate = ref('');

const emit = defineEmits(['add-todo']);
const addTodoHandler = () => {
  // 할일이 3글자 이상인 경우에만
  if (todo.value.length >= 3) {
    //부모 컴포넌트에 이벤트 전달(방출)
    let todoObj = { todo: todo.value, duedate: duedate.value };

    emit('add-todo', todoObj);
    todo.value = '';
    duedate.value = ''; //input 초기화
    return; // 메소드 종료 : 밑에 코드 수행 X
  }

  alert('할 일은 3글자 이상 입력해주세요.');

  //단순 전달 시 태그에서 $emit으로 전달가능
};

//emits: ['addTodo'],
//이벤트 방출 검사
//-> 부모 컴포넌트로 전달되는 이벤트가
//  addTodo가 맞는지 검사
</script>
