<template>
	<div>
		<!-- {{ todos }} -->
		<!-- <Todos v-model="todos"></Todos> -->
		<Todos></Todos>

		<!-- <button @click="addTodo">ajouter une tahce</button> -->
	</div>
</template>

<script>
import Todos from './components/Todos'
export default {

	data() {
		return {
			todos: [{
				name: 'demo',
				completed: true
			}]
		}
	},
	name: 'App',
	components: {
		Todos
	},
	methods: {
		addTodo() {
			this.todos.push({
				name: "jean",
				completed: false
			})
		}
	}
}
</script>


