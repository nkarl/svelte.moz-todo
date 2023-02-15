<!-- Todos.svelte -->
<script>
    import FilterButton from "./FilterButton.svelte";
    import Todo from "./Todo.svelte";

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

    let filterState = "all";
    const filterTodos = (filterState, todos) =>
        filterState === "active"
            ? todos.filter((t) => !t.completed)
            : filterState === "completed"
            ? todos.filter((t) => t.completed)
            : todos;
</script>

<!-- MARK UP START HERE -->
<div class="todoapp stack-large">
    <!-- NewTodo Item -->
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

    <!-- Filter Button -->
    <FilterButton bind:filterState />

    <!-- Status of Todo Items -->
    <h2 id="list-heading">
        {completedTodos} out of {totalTodos} items completed
    </h2>

    <!-- To-dos -->
    <ul
        role="list"
        class="todo-list stack-large"
        aria-labelledby="list-heading"
    >
        {#each filterTodos(filterState, todos) as todo (todo.id)}
            <li class="todo">
                <!-- To-do Item -->
                <Todo {todo} on:remove={(e) => removeTodo(e.detail)} />
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
