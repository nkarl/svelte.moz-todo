<!-- Todo.svelte -->
<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    export let todo;

    let editing = false; // track editing mode
    let name = todo.name; // hold the name of the To-do Item being edited

    /*
     * Update the To-do Item
     */
    function update(updatedTodo) {
        todo = { ...todo, ...updatedTodo }; // applies updates to To-do Item
        dispatch("update", todo);
    }

    function onCancel() {
        name = todo.name; // restores the name to its initial value, and
        editing = false; // sets editing mode to exit.
    }

    function onSave() {
        update({ name }); // updates the To-do Item's name, and
        editing = false; // sets editing mode to exit.
    }

    function onRemove() {
        dispatch("remove", todo); // emits remove event.
    }

    function onEdit() {
        editing = true; // sets editing mode to true.
    }

    function onToggle() {
        update({ completed: !todo.completed }); // changes checkbox state.
    }
</script>

<!-- MARK UP STARTS HERE -->
<div class="stack-small">
    {#if editing}
        <!-- markup for editing to-do: label, input text, Cancel and Save Button -->
    {:else}
        <!-- markup for displaying to-do: checkbox, label, Edit and Delete Button -->
        <div class="c-cb">
            <input
                type="checkbox"
                id="todo-{todo.id}"
                on:click={onToggle}
                checked={todo.completed}
            />
            <label for="todo-{todo.id}" class="todo-label">
                {todo.name}
            </label>
        </div>
        <div class="btn-group">
            <button type="button" class="btn" on:click={onEdit}>
                Edit <span class="visually-hidden">{todo.name}</span>
            </button>
            <button type="button" class="btn btn__danger" on:click={onRemove}>
                Delete <span class="visually-hidden">{todo.name}</span>
            </button>
        </div>
    {/if}
</div>
