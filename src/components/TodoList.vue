<template>
  <div class="row">
    <div class="col">
      <ul class="list-group">
        <TodoListItem
          v-for="todoItem in todoList"
          :key="todoItem.id"
          :todoItem="todoItem"
          :todo="todoItem.todo"
          :duedate="todoItem.duedate"
          :completed="todoItem.completed"
          @delete-todo="$emit('delete-todo', $event)"
          @checkbox-completed="$emit('toggle-completed', $event)"
        />
        <!-- 이벤트 방출 시 전달되는 데이터는 $event로 수신 가능 -->
      </ul>
    </div>
  </div>
</template>
<script setup>
import TodoListItem from './TodoListItem.vue';

// props : 부모 컴포넌트에서 전달된 데이터
const props = defineProps({ todoList: { type: Array, required: true } });
</script>
