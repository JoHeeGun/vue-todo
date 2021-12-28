<template>
  <div class="todo-app">
    <div class="todo-app__actions">
      <!-- FILTERS -->
      <div class="filters">
        <router-link to="all" tag="button">
          모든 항목 ({{ total }})
        </router-link>
        <router-link to="active" tag="button">
          해야 할 항목 ({{ activeCount }})
        </router-link>
        <router-link to="completed" tag="button">
          완료된 항목 ({{ completedCount }})
        </router-link>
      </div>

      <!-- ACTIONS -->
      <div class="actions clearfix">
        <label class="float--left">
          <input v-model="allDone" type="checkbox" />
          <span class="icon"><i class="material-icons">전체선택</i></span>
        </label>
        <button class="btn btn--danger float--left" @click="clearCompleted">
          <i class="material-icons">선택된 항목 삭제</i>
        </button>
      </div>
    </div>

    <!-- LIST -->
    <div class="todo-app__list">
      <todo-item v-for="todo in filteredTodos" :key="todo.id" :todo="todo" />
    </div>

    <!-- INSERT -->
    <todo-creator class="todo-app__creator" />
  </div>
</template>

<script>
import { mapGetters, mapMutations, mapActions } from 'vuex';
import TodoCreator from '../components/TodoCreator.vue';
import TodoItem from '../components/TodoItem';
export default {
  name: 'TodoApp',
  components: {
    TodoCreator,
    TodoItem,
  },
  computed: {
    // ...mapState('todoApp', [
    //   'db',
    //   'todos'
    // ]),
    ...mapGetters('todoApp', [
      'filteredTodos',
      'total',
      'activeCount',
      'completedCount',
    ]),
    allDone: {
      get() {
        // 전체 항목 개수와 완료된 항목 개수가 일치하고 항목 개수가 1개 이상인 경우.
        return this.total === this.completedCount && this.total > 0;
      },
      set(checked) {
        this.completeAll(checked);
      },
    },
  },
  watch: {
    $route() {
      this.updateFilter(this.$route.params.id);
    },
  },
  created() {
    this.initDB();
  },
  methods: {
    ...mapMutations('todoApp', ['updateFilter']),
    ...mapActions('todoApp', ['initDB', 'completeAll', 'clearCompleted']),
  },
};
</script>
