<template>
  <div>
    <section class="todoapp">
			<header class="header">
				<h1>todos</h1>
				<input v-model="todo" @keyup.enter="addTodo" class="new-todo" autofocus autocomplete="off" placeholder="What needs to be done?">
			</header>
			<section class="main" v-if="isShowTodos">
				<input v-model="allCompleted" id="toggle-all" class="toggle-all" type="checkbox">
				<label for="toggle-all">Mark all as complete</label>
				<ul class="todo-list">
					<li v-for="(item, index) in filteredTodos" :key="index" :class="displayMode(item)">
						<div class="view">
							<input v-model="item.completed" class="toggle" type="checkbox">
							<label @dblclick="editTodo(item)">{{ item.title }}</label>
							<button @click="removeTodo(item)" class="destroy"></button>
						</div>
						<input v-model="item.title" @keyup.enter="updateTodo(item)" @blur="updateTodo(item)" @keyup.esc="cancelTodo(item)" class="edit" type="text">
					</li>
				</ul>
			</section>
			<footer v-if="isShowTodos" class="footer">
				<span class="todo-count">
					<strong>{{ activeTodosCount }}</strong> item left
				</span>
				<ul class="filters">
					<li><a @click="allTodos" :class="clickedCSS('all')" href="javascript:void(0)">All</a></li>
					<li><a @click="activeTodos" :class="clickedCSS('active')" href="javascript:void(0)">Active</a></li>
					<li><a @click="completedTodos" :class="clickedCSS('completed')" href="javascript:void(0)">Completed</a></li>
				</ul>
				<button @click="clearCompletedTodos" v-if="isShowClearCompleted" class="clear-completed">
					Clear completed
				</button>
			</footer>
		</section>
		<footer class="info">
			<p>Double-click to edit a todo</p>
			<p>Written by <a href="https:oomusou.io">Sam Xiao</a></p>
			<p>Part of <a href="http://todomvc.com">TodoMVC</a></p>
		</footer>
  </div>
</template>

<script>

/** 新增 todo */
const addTodo = function() {
  if (!this.todo) return;

  this.todos = [...this.todos, { title: this.todo, edit: false, completed: false }];
  this.todo = '';
};

/** 刪除 todo */
const removeTodo = function(item) {
  const cb = x => !(x.title === item.title);
  this.todos = this.todos.filter(cb);
};

/** 編輯 todo */
const editTodo = function(item) {
  this.oldTitle = item.title;
  item.edit = true;
};

/** 顯示模式的 CSS */
const displayMode = function(item) {
  return { editing: item.edit, completed: item.completed };
};

/** 儲存 todo */
const updateTodo = function(item) {
  item.edit = false;
};

/** 取消儲存 todo */
const cancelTodo = function(item) {
  item.title = this.oldTitle;
  item.edit = false;
};

/** 顯示全部 todo */
const allTodos = function() {
  this.filterCb = x => x;
  this.filterMode = 'all';
};

/** 顯示未完成 todo */
const activeTodos = function() {
  this.filterCb = x => !x.completed;
  this.filterMode = 'active';
};

/** 顯示已完成 todo */
const completedTodos = function() {
  this.filterCb = x => x.completed;
  this.filterMode = 'completed';
};

/** 已經過過濾的 todo */
const filteredTodos = function() {
  return this.todos.filter(this.filterCb);
};

/** 回傳按下過濾 button 該顯示的 CSS */
const clickedCSS = function(mode) {
  return { selected:  this.filterMode === mode };
};

/** 刪除已完成 todo */
const clearCompletedTodos = function() {
  const cb = x => !x.completed;
  this.todos = this.todos.filter(cb);
};

/** 未完成 todo 數量 */
const activeTodosCount = function() {
  const cb = x => !x.completed;
  return this.todos.filter(cb).length;
};

/** 使全部為 completed */
const allCompleted = {
  get: function() {
    return !this.activeTodosCount;
  },
  set: function() {
    const cb = x => ({ title: x.title, edit: x.edit, completed: true });
    this.todos = this.todos.map(cb);
  },
};

/** 是否顯示 todos */
const isShowTodos = function() {
  return this.todos.length;
};

/** 是否顯示 Clear Computed */
const isShowClearCompleted = function() {
  return this.todos.length > this.activeTodosCount;
};

export default {
  name: 'app',
  data: function() {
    return {
      /** 新增的 todo */
      todo: '',
      /** 所有 todos */
      todos: [],
      /** 編輯前的 title */
      oldTitle: '',
      /** 過濾 callback */
      filterCb: x => x,
      /** 目前的 filter mode */
      filterMode: 'all'
    };
  },
  computed: {
    filteredTodos,
    activeTodosCount,
    allCompleted,
    isShowTodos,
    isShowClearCompleted
  },
  methods: {
    addTodo,
    removeTodo,
    editTodo,
    displayMode,
    updateTodo,
    cancelTodo,
    allTodos,
    activeTodos,
    completedTodos,
    clickedCSS,
    clearCompletedTodos,
  }
}
</script>