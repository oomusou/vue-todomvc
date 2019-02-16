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
					<li v-for="(item, index) in todos" :key="index"  class="todo">
						<div class="view">
							<input class="toggle" type="checkbox">
							<label>{{ item.title }}</label>
							<button class="destroy"></button>
						</div>
						<input class="edit" type="text">
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

  this.todos = [...this.todos, { title: this.todo }];
  this.todo = '';
};

export default {
  name: 'app',
  data: () => ({
    /** 新增的 todo */
    todo: '',
    /** 所有 todos */
    todos: [],
  }),
  methods: {
    addTodo,
  }
}
</script>