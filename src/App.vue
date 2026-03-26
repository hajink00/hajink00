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
        <TodoList :todoList="todoList" @delete-todo="deleteTodo" />
        <!-- :todolist -> 자식에게 todolist라는 속성으로 데이터 전달 -->
        <!-- @delete-todo : 자식으로부터 delete-todo라는 이벤트가 감지되면 
            "deleteTodo": 해당 메소드를 호출한다. -->
        <!-- deleteTodo == deleteTodo($event)
          -> 미작성 시 Vue 내부적으로 추가 -->
      </div>
    </div>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue';
import InputTodo from './components/InputTodo.vue';
let ts = new Date().getTime();
export default {
  name: 'App',
  components: { InputTodo, TodoList },
  data() {
    return {
      todoList: [
        { id: ts, todo: '자전거 타기', completed: false },
        { id: ts + 1, todo: '딸과 공원 산책', completed: true },
        { id: ts + 2, todo: '일요일 애견 카페', completed: false },
        { id: ts + 3, todo: 'Vue 원고 집필', completed: false },
      ],
    };
  },
  methods: {
    //InputTodo 컴포넌트에서 받은 add-todo 이벤트 실행시 호출할 addTodo 메서드
    addTodo(todo) {
      if (todo.length >= 2) {
        this.todoList.push({
          id: new Date().getTime(),
          todo: todo,
          completed: false,
        });
      }
    },
    // 할일 삭제
    deleteTodo(id) {
      let index = this.todoList.findIndex((item) => id === item.id);
      this.todoList.splice(index, 1);
    },
  },
};
</script>
