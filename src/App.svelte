<script>
	let todos = [];
	
	try {
		// First, get the todos string from localStorage using the key "todos"
		let todosString = localStorage.getItem("todos");

		// Then, parse the string into an array using JSON.parse
		todos = JSON.parse(todosString);
		
		if(todos === null) {
			todos = [];
		}
	} catch(e) {
		console.log("Can't get todos");
	}

	let newTodo = "";

	function removeFromList(todo) {
		if(todos.length === 0) {
			return;
		}

		let indexToRemove = 0;
		for(let i = 0; i < todos.length; i++) {
			if(todos[i].id === todo.id) {
				indexToRemove = i;
			}
		}

		todos.splice(indexToRemove, 1);
		todos = todos;
	}

	function toggleTodoDone(id) {
		let todoIndex = todos.findIndex(todo => todo.id === id); // find the index of the todo with the given id
		if (todoIndex !== -1) { // check if the todo exists
			todos[todoIndex].done = !todos[todoIndex].done; // toggle the "done" property of the todo
		}
		save();
	}


	function addTodo() {
		todos.push({content: newTodo, done: false, id: Math.floor(Math.random() * 10000)});
		todos = todos;
		newTodo = "";
		save();
	}

	function save() {
		// First, convert your todos array to a string using JSON.stringify
		let todosString = JSON.stringify(todos);

		// Then, save the string to localStorage under the key "todos"
		localStorage.setItem("todos", todosString);
	}

</script>

<div class="wrapper">
	{#if todos && todos.length > 0}
		<ul class="to-do-list">
			{#each todos as todo}
				<li class="item">
					<!-- svelte-ignore a11y-click-events-have-key-events -->
					<div on:click={() => removeFromList({todo})} class="delete-icon">
						<i class="fa-solid fa-trash"></i>
					</div>
					<!-- svelte-ignore a11y-click-events-have-key-events -->
					{#if todo.done} 
					<div class="item-content" on:click={() => toggleTodoDone(todo.id)} style="text-decoration: line-through;">
						{todo.content}
					</div>
					{:else}
					<div class="item-content" on:click={() => toggleTodoDone(todo.id)}>
						{todo.content}
					</div>
					{/if}
				</li>
			{/each}
		</ul>
	{/if}

	<input type="text" placeholder="New todo..." required  bind:value={newTodo} on:change={addTodo}/>
</div>

<style>
	.wrapper {
		display: flex;
		align-items: center;
		background-color: #FFD599;
		flex-direction: column;
		width: 40vw;
		padding: 10px;
		border-radius: 10px;
	}

	.to-do-list {
		list-style: none;
		padding: 0;
		margin: 0;
		font-size: 20px;
	}

	.item {
		margin: 0;
		display: flex;
		width: 30vw;
		justify-content: start;
		padding: 10px;
		cursor: pointer;
	}

	.delete-icon {
		margin-right: 10px;
		cursor: pointer;
	}

	input {
      border: none;
      border-radius: 5px;
      padding: 10px;
      font-size: 16px;
      background-color: #FFCC85;
	  width: 35vw;
    }
</style>