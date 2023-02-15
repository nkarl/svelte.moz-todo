<script>
    export let filterState = "all";
</script>

<div class="filters btn-group stack-exception">
    <button
        class="btn toggle-btn"
        class:btn__primary={filterState === "all"}
        aria-pressed={filterState === "all"}
        on:click={() => (filterState = "all")}
    >
        <span class="visually-hidden">Show</span>
        <span>All</span>
        <span class="visually-hidden">tasks</span>
    </button>
    <button
        class="btn toggle-btn"
        class:btn__primary={filterState === "active"}
        aria-pressed={filterState === "active"}
        on:click={() => (filterState = "active")}
    >
        <span class="visually-hidden">Show</span>
        <span>Active</span>
        <span class="visually-hidden">tasks</span>
    </button>
    <button
        class="btn toggle-btn"
        class:btn__primary={filterState === "completed"}
        aria-pressed={filterState === "completed"}
        on:click={() => (filterState = "completed")}
    >
        <span class="visually-hidden">Show</span>
        <span>Completed</span>
        <span class="visually-hidden">tasks</span>
    </button>
</div>
