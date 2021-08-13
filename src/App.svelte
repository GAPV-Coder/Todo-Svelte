<script>

	import {Toast} from 'bootstrap'

	let toastEl;
	$: console.log(toastEl);

	let opc = {texto: '', color: ''}

	// Guardar la inicialización del Toast
	let instancia;
	$: if (toastEl) {
		instancia = new Toast(toastEl)
	}

	const mostrarToast = (texto, color) => {
		opc = {texto, color}
		instancia.show()
	}

	let todos = []
	let todo = {id: "", texto: "", estado: false};

	// Alojamiento de datos en localStorage

	if (localStorage.getItem("todos")) {
  		todos = JSON.parse(localStorage.getItem("todos"));
}

$: localStorage.setItem("todos", JSON.stringify(todos));

	// Funciones CRUD

	const addTodos = () => {
		if (!todo.texto.trim()) {
			console.log('texto vacio');
			todo.texto = "";
			return
		}

		todo.id = Date.now();
		todos = [...todos, todo];
		console.log(todos);
		todo = {id: "", texto: "", estado: false};

		mostrarToast('Todo agregado satisfactoriamente', 'bg-primary');
	}

	const delTodo = id => {
		todos = todos.filter((item) => item.id !== id);

		mostrarToast('Todo eliminado satisfactoriamente', 'bg-danger');
	}

	
	const editTodo = id => {
		todos = todos.map(item =>
		item.id === id ?
		{...item, estado: !item.estado}
		: item
		)
		console.log(todos);
		mostrarToast('Todo editado satisfactoriamente', 'bg-warning');
	}
	
	const classIcono = (valor) => (valor ? "bi bi-arrow-clockwise" : "bi bi-check2");
	const classEstado = (valor) => valor ? "btn btn-sm btn-success" : "btn btn-sm btn-warning";

</script>

<div class="container">
	<h1 class="display-5 my-5">Add ToDo Svelte</h1>
	<form on:submit|preventDefault={addTodos}>
		<input type="text" placeholder="Enter para agregar tarea" class="form-control shadow border-0" bind:value={todo.texto} />
	</form>
	{#each todos as item}
		<div class="shadow my-3 p-3 lead">
			<p class={item.estado ? 'text-decoration-line-through' : ''}>
				{item.texto}
			</p>
			<button class={classEstado(item.estado)} on:click={editTodo(item.id)}>
				<i class={classIcono(item.estado)} />
			</button>
			<button class="btn btn-sm btn-danger" on:click={delTodo(item.id)}>
				<i class="bi bi-trash-fill"></i>
			</button>
		</div>
	{/each}

	<!-- Toast Bootstrap -->
	<div class="toast-container position-absolute p-3 top-0 end-0">
		<div bind:this={toastEl} 
		class="toast align-items-center text-white {opc.color} border-0" 
		role="alert" 
		aria-live="assertive" 
		aria-atomic="true"
		>
			<div class="d-flex">
				<div class="toast-body">
					{opc.texto}
				</div>
			<button type="button" class="btn-close btn-close-white me-2 m-auto" data-bs-dismiss="toast" aria-label="Close"></button>
			</div>
		</div>
	</div>

</div>