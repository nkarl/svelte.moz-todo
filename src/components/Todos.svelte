<!-- Todos.svelte -->
<script>
    export let todos = [];
    $: totalTodos = todos.length;
    $: completedTodos = todos.filter((todo) => todo.completed).length;

    function removeTodo(todo) {
        todos = todos.filter((t) => t.id !== todo.id);
    }

    let newTodoName = "";
    function addTodo() {
        todos = [
            ...todos,
            { id: newTodoId, name: newTodoName, completed: false },
        ];
        newTodoName = "";
    }
    let newTodoId;
    $: {
        if (totalTodos === 0) {
            newTodoId = 1;
        } else {
            newTodoId = Math.max(...todos.map((t) => t.id)) + 1;
        }
    }

    let status = "all";
    const filterTodos = (filter, todos) =>
        filter === "active"
            ? todos.filter((t) => !t.completed)
            : filter === "completed"
            ? todos.filter((t) => t.completed)
            : todos;
</script>

<div class="todoapp stack-large">
    <!-- NewTodo -->
    <form on:submit|preventDefault={addTodo}>
        <h2 class="label-wrapper">
            <label for="todo-0" class="label__lg">
                What needs to be done?
            </label>
        </h2>
        <input
            bind:value={newTodoName}
            type="text"
            id="todo-0"
            autocomplete="off"
            class="input input__lg"
        />
        <button type="submit" disabled="" class="btn btn__primary btn__lg">
            Add
        </button>
    </form>

    <!-- Filter -->
    <div class="filters btn-group stack-exception">
        <button
            class="btn toggle-btn"
            class:btn__primary={status === "all"}
            aria-pressed={status === "all"}
            on:click={() => (status = "all")}
        >
            <span class="visually-hidden">Show</span>
            <span>All</span>
            <span class="visually-hidden">tasks</span>
        </button>
        <button
            class="btn toggle-btn"
            class:btn__primary={status === "active"}
            aria-pressed={status === "active"}
            on:click={() => (status = "active")}
        >
            <span class="visually-hidden">Show</span>
            <span>Active</span>
            <span class="visually-hidden">tasks</span>
        </button>
        <button
            class="btn toggle-btn"
            class:btn__primary={status === "completed"}
            aria-pressed={status === "completed"}
            on:click={() => (status = "completed")}
        >
            <span class="visually-hidden">Show</span>
            <span>Completed</span>
            <span class="visually-hidden">tasks</span>
        </button>
    </div>

    <!-- TodosStatus -->
    <h2 id="list-heading">
        {completedTodos} out of {totalTodos} items completed
    </h2>

    <!-- To-dos -->
    <ul
        role="list"
        class="todo-list stack-large"
        aria-labelledby="list-heading"
    >
        {#each filterTodos(status, todos) as todo (todo.id)}
            <li class="todo">
                <div class="stack-small">
                    <div class="c-cb">
                        <input
                            type="checkbox"
                            id="todo-{todo.id}"
                            checked={todo.completed}
                        />
                        <label for="todo-{todo.id}" class="todo-label">
                            {todo.name}
                        </label>
                    </div>
                    <div class="btn-group">
                        <button type="button" class="btn">
                            Edit <span class="visually-hidden">{todo.name}</span
                            >
                        </button>
                        <button
                            type="button"
                            class="btn btn__danger"
                            on:click={() => removeTodo(todo)}
                        >
                            Delete <span class="visually-hidden"
                                >{todo.name}</span
                            >
                        </button>
                    </div>
                </div>
            </li>
        {:else}
            <li>Nothing to do here!</li>
        {/each}
    </ul>

    <hr />

    <!-- MoreActions -->
    <div class="btn-group">
        <button type="button" class="btn btn__primary">Check all</button>
        <button type="button" class="btn btn__primary">Remove completed</button>
    </div>
</div>
