<template>
  <li
    class="list-group-item"
    :class="{ 'list-group-item-success': todoItem.completed }"
  >
    <!-- :class="{클래스명 : 조건}" : 조건이 true면 클래스 추가 -->
    <!-- <input type="checkbox" class="pointer me-3" v-model="todoItem.completed" /> -->
    <!-- 객체 props에 v-model을 쓰면 부모 데이터도 같이 바뀐다.
    -> 하지만 props는 읽기 전용이 원칙이라 권장 X
    -> 해결 : 자식 컴포넌트에서 $emit으로 부모에게 알려서 바꾸자! -->

    <input
      type="checkbox"
      class="pointer me-3"
      @click="emit('checkbox-completed', todoItem.id)"
      :checked="todoItem.completed"
    />

    <span class="pointer" :class="{ 'todo-done': todoItem.completed }">
      {{ todoItem.todo }}
      {{ todoItem.completed ? '(완료)' : '' }}
      <!-- 삼항연산자 : 조건식 ? true : false -->
    </span>
    <span
      class="float-end badge bg-secondary pointer"
      @click="emit('delete-todo', todoItem.id)"
      >삭제</span
    >
    <span
      class="float-end badge"
      :class="todoItem.duedate ? 'bg-danger' : 'bg-success'"
      >{{ todoItem.duedate ? todoItem.duedate : '마감일 없음' }}</span
    >
  </li>
</template>
<script setup>
//prop : 부모 컴포넌트에서 전달된 데이터 수신
const props = defineProps({
  todoItem: { type: Object, required: true },
});
const emit = defineEmits(['checkbox-completed', 'delete-todo']);
</script>
