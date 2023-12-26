<template>
	<section class="todoapp">
		<header class="header">
			<h1>Todos</h1>
			<input type="text" class="new-todo" placeholder="ajouter une tache" v-model="newTodo" @keyup.enter="addTodo">
		</header>
		<div class="main">
			<input id="toggle-all" class="toggle-all" type="checkbox" v-model="allDone">
			<label for="toggle-all">Mark all as complete</label>
			<ul class="todo-list">
				<li class="todo" v-for="todo in filteredTodos"
					:class="{ completed: todo.completed, editing: todo === editing }">
					<div class="view">
						<input type="checkbox" v-model="todo.completed" class="toggle">
						<label @dblclick="editTodo(todo)">{{ todo.name }}</label>
						<button class="destroy" @click.prevent="deleteTodo(todo)"></button>
					</div>
					<input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit" @blur="doneEdit()"
						@keyup.esc="cancelEdit()" v-focus="todo === editing">
				</li>
			</ul>
		</div>
		<footer class="footer" v-show="hasTodos">
			<span class="todo-count"><strong>{{ remaining }}</strong> tache a faire</span>
			<ul class="filters">
				<li><a href="#" :class="{ selected: filter === 'all' }" @click.prevent="filter = 'all'">Toutes lestaches</a>
				</li>
				<li><a href="#" :class="{ selected: filter === 'todo' }" @click.prevent="filter = 'todo'">a faire</a></li>
				<li><a href="#" :class="{ selected: filter === 'done' }" @click.prevent="filter = 'done'">faites</a></li>
			</ul>
			<button class="clear-completed" @click.prevent="deleteCompleted" v-show="completed">clear
				completed
			</button>
		</footer>
	</section>
</template>

<script>
import Vue from 'vue'
export default {
	watch: {
		value(value) {
			this.todos = value
		}
	},
	props: {
		value: { type: Array, default() { return [] } }
	},
	data() {
		return {
			todos: this.value,
			newTodo: '',
			filter: 'all',
			editing: null,
			oldTodo: ''
		};
	},

	methods: {
		addTodo() {
			this.todos.push({
				completed: false,
				name: this.newTodo
			});
			this.newTodo = '';
		},
		deleteTodo(todo) {
			this.todos = this.todos.filter(i => i !== todo)
		},
		deleteCompleted() {
			this.todos = this.todos.filter(todo => !todo.completed)
		},
		editTodo(todo) {

			this.editing = todo
			this.oldTodo = todo.name
		},
		doneEdit() {
			this.editing = null
		},
		cancelEdit(todo) {
			this.editing.name = oldTodo,
				this.doneEdit()
		}
	},
	computed: {
		allDone: {
			get() {
				return this.remaining === 0
			},
			set(value) {
				this.todos.forEach(todo => {
					todo.completed = value
				})
			}

		},
		hasTodos() {
			return this.todos.length > 0
		},
		remaining() {
			return this.todos.filter(todo => !todo.completed).length;
		},
		completed() {
			return this.todos.filter(todo => todo.completed).length;
		},
		filteredTodos() {
			if (this.filter === 'todo') {
				return this.todos.filter(todo => !todo.completed);
			} else if (this.filter === 'done') {
				return this.todos.filter(todo => todo.completed);
			}
			return this.todos;
		}
	},
	directives: {
		focus: function (el, value) {
			if (value) {
				Vue.nextTick(_ => {
					el.focus()
				})
			}
		}
	}
};
</script>

<style src="./todos.css"></style>
