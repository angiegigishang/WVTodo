<template>
	<section class="real-app">
		<input type="text"
		       class="add-input"
		       autofocus="autofocus"
		       placeholder="接下去要做什么?"
		       @keyup.enter="addTodo"
		>
		<item :todo="todo"
		       v-for="todo in filteredTodos"
		       :key="todo.id"
		       @del="deleteTodo"></item>
		<tabs :filter="filter" 
		      :todos="todos" 
		      @toggle="toggleFilter"
		      @clearAllCompleted="clearAllCompleted"></tabs>
	</section>
</template>

<script>
import Item from './item.vue'
import Tabs from './tabs.vue'
export default {
	data() {
		return {
			todos: [],
			filter: 'all',
			id: 0
		}
	},
	components: {
		Item,
		Tabs
	},
	computed: {
		filteredTodos() {
			if(this.filter === 'all') {
				return this.todos
			}
			const completed = this.filter === 'completed'
			return this.todos.filter(todo => completed === todo.completed)
		}
	},
	methods: { 
		addTodo(e) {
			this.todos.unshift({
				id: this.id++,
				content: e.target.value.trim(),
				completed: false
			})
			e.target.value = ''
		},
		deleteTodo(id) {
			this.todos.splice(this.todos.findIndex(todo => todo.id === id), 1)
		},
		toggleFilter(state) {
			this.filter = state
		},
		clearAllCompleted() {
			this.todos = this.todos.filter(todo => !todo.completed)
		}
	}
}
</script>

<style>
.real-app {
	width: 600px;
	margin: 0 auto;
	box-shadow: 0 0 5px #666;   
}	
.add-input {
	position: relative;
	width: 100%;
	font-size: 24px;
	font-family: inherit;
	font-weight: inherit;
	line-height: 1.4em;
	border: 0;
	outline: none;
	color: inherit;
	padding: 6px;
	border: 1px solid #999;
	box-shadow: inset 0 -1px 5px 0 grey;
	box-sizing: border-box;
	font-smoothing: antialiased;
	padding: 16px 16px 16px 60px;
	border: none;
	box-shadow: inset 0 -2px 1px grey;
}
</style>