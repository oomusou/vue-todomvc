<template>
  <div>
    <section class="todoapp">
			<header class="header">
				<h1>todos</h1>
				<input v-model="todo" @keyup.enter="addTodo" class="new-todo" autofocus autocomplete="off" placeholder="What needs to be done?">
			</header>
			<section class="main">
				<input id="toggle-all" class="toggle-all" type="checkbox">
				<label for="toggle-all">Mark all as complete</label>
				<ul class="todo-list">
					<li v-for="(item, index) in todos" :key="index" :class="displayMode(item)">
						<div class="view">
							<input v-model="item.completed" class="toggle" type="checkbox">
							<label @dblclick="editTodo(item)">{{ item.title }}</label>
							<button @click="removeTodo(item)" class="destroy"></button>
						</div>
						<input v-model="item.title" @keyup.enter="updateTodo(item)" @blur="updateTodo(item)" @keyup.esc="cancelTodo(item)" class="edit" type="text">
					</li>
				</ul>
			</section>
			<footer class="footer">
				<span class="todo-count">
					<strong></strong> left
				</span>
				<ul class="filters">
					<li><a href="#">All</a></li>
					<li><a href="#">Active</a></li>
					<li><a href="#">Completed</a></li>
				</ul>
				<button class="clear-completed">
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

export default {
  name: 'app',
  data: () => ({
    /** 新增的 todo */
    todo: '',
    /** 所有 todos */
    todos: [],
    /** 編輯前的 title */
    oldTitle: '',
  }),
  methods: {
    addTodo,
    removeTodo,
    editTodo,
    displayMode,
    updateTodo,
    cancelTodo,
  }
}
</script>