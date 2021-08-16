<script>
	import { todos } from './stores'
	import TodoAdd from "./components/TodoAdd.svelte";
	import Todo from './components/Todo.svelte';
	import Toast from './components/Toast.svelte';
	if (localStorage.getItem("todos")) {
	  todos.local(JSON.parse(localStorage.getItem("todos")))
	}
	$: localStorage.setItem("todos", JSON.stringify($todos));
</script>

<div class="container">
	<h1 class="display-5 my-3">ToDo Svelte</h1>
	<TodoAdd />
	{#each $todos as item}
	<Todo {item} />
	{/each}
	<Toast />
</div>