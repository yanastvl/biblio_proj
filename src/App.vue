<template>
  <div>
      <TodoList/>
      <!-- <ModalWindow/> -->
  </div>
</template>

<script>
import ModalWindow from './components/ModalWindow.vue';
import TodoList from './components/TodoList.vue';

export default {
    components: { ModalWindow, TodoList}
};

</script>