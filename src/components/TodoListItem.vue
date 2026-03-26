<template>
  <li
    class="list-group-item"
    :class="{ 'list-group-item-success': todoItem.completed }"
  >
    <!-- :class="{클래스명 : 조건}" : 조건이 true면 클래스 추가 -->
    <input type="checkbox" class="pointer me-3" v-model="todoItem.completed" />
    <span class="pointer" :class="{ 'todo-done': todoItem.completed }">
      {{ todoItem.todo }}
      {{ todoItem.completed ? '(완료)' : '' }}
      <!-- 삼항연산자 : 조건식 ? true : false -->
    </span>
    <span
      class="float-end badge bg-secondary pointer"
      @click="$emit('delete-todo', todoItem.id)"
      >삭제</span
    >
  </li>
</template>
<script>
export default {
  name: 'TodoListItem',
  //prop : 부모 컴포넌트에서 전달된 데이터 수신
  props: {
    todoItem: { type: Object, required: true },
  },
};
</script>
