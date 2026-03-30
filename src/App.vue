<template>
  <div id="app" class="container">
    <div class="card card-body bg-light">
      <div class="title">:: Todolist App</div>
    </div>
    <div class="card card-default panel-borderless">
      <div class="card-body">
        <InputTodo @add-todo="addTodo" />
        <!-- add-todo 이벤트 발생시 addTodo 메서드 실행  -->
        <!-- 목록 컴포넌트
            props를 이용해서 todoList를 자식 컴포넌트로 전달 -->
        <TodoList
          :todoList="todoListSorted"
          @delete-todo="deleteTodo"
          @toggle-completed="toggleCompleted"
        />
        <!-- :todolist -> 자식에게 todolist라는 속성으로 데이터 전달 -->
        <!-- @delete-todo : 자식으로부터 delete-todo라는 이벤트가 감지되면 
            "deleteTodo": 해당 메소드를 호출한다. -->
        <!-- deleteTodo == deleteTodo($event)
          -> 미작성 시 Vue 내부적으로 추가 -->
      </div>
    </div>
  </div>
</template>

<script setup>
import TodoList from './components/TodoList.vue';
import InputTodo from './components/InputTodo.vue';
import { reactive, computed } from 'vue';

let ts = new Date().getTime();

const todoList = reactive([
  { id: ts, todo: '자전거 타기', completed: false },
  { id: ts + 1, todo: '딸과 공원 산책', completed: true },
  { id: ts + 2, todo: '일요일 애견 카페', completed: false },
  { id: ts + 3, todo: 'Vue 원고 집필', completed: false },
]);

//마감일 순 정렬
const todoListSorted = computed(() => {
  //원본 데이터 가져오기
  return [...todoList].sort((a, b) => {
    // 완료 항목 최하단 배치
    if (a.completed !== b.completed) {
      return a.completed ? 1 : -1;
    }

    //마감일 비교
    const dateA = a.duedate || '9999-99-99';
    const dateB = b.duedate || '9999-99-99';

    if (dateA < dateB) return -1;
    if (dateA > dateB) return 1;
    return 0;
  });
});

//InputTodo 컴포넌트에서 받은 add-todo 이벤트 실행시 호출할 addTodo 메서드
const addTodo = (todoObj) => {
  if (todoObj.todo.length >= 2) {
    todoList.push({
      id: new Date().getTime(),
      todo: todoObj.todo,
      duedate: todoObj.duedate,
      completed: false,
    });
  }
}; //add todo

// 할일 삭제
// id가 일치하는 요소의 index 변수에 담기

const deleteTodo = (id) => {
  const deleteConfirm = window.confirm('할일을 삭제하시겠습니까?');

  if (deleteConfirm) {
    let index = todoList.findIndex((item) => id === item.id);
    //console.log(index);

    // splice(시작index, 삭제개수)
    todoList.splice(index, 1);

    alert('할일이 삭제되었습니다!');
  } else {
    alert('삭제 취소');
  }
}; //deleteTodo

//체크박스 선택
const toggleCompleted = (id) => {
  // 배열.findIndex((item)) => 조건) : 조건이 true가 되는 첫번째 요소의 index 반환
  // 없는 경우 -1 반환
  let index = todoList.findIndex((item) => id == item.id);
  // == (동등 비교) : 값만 비교
  // === (동일 비교) : 값 + 타입까지 비교
  // console.log(index);

  // console.log(todoList[index]);
  // console.log(todoList[index].completed);
  // console.log(!todoList[index].completed);
  // !true/false : true -> false, false -> true로 변경

  // 기존의 completed 값을 반대로 변경
  todoList[index].completed = !todoList[index].completed;
}; // toggleCompleted

/*
 * 할일 추가
 * @param todo InputTodo에서 전달된 todo(할일 내용)
 */

// addTodo(todo) {
//   console.log(todo);
//   todoList.push({
//     id: Date.now(),
//     todo: todo,
//     completed: false,
//   });
// },
</script>
