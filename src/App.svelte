<script>
  import uuid from "./utils.js";

  const FILTER_TITLES = ["All", "Active", "Completed"];

  let todos = [];
  let newTodo = "";
  let filter = "All";

  $: numActive = todos.filter(todo => todo.completed === false).length;
  $: filteredTodos =
    filter === "All"
      ? todos
      : filter === "Active"
      ? todos.filter(todo => todo.completed === false)
      : todos.filter(todo => todo.completed !== false);

  function addTodo(event) {
    event.preventDefault();

    todos = [
      ...todos,
      {
        id: uuid(),
        completed: false,
        text: newTodo
      }
    ];

    newTodo = "";
  }

  function deleteTodo(id) {
    todos = todos.filter(todo => todo.id !== id);
  }

  function clearCompleted() {
    todos = todos.filter(todo => todo.completed === false);
  }
</script>

<section class="todoapp">
  <header class="header">
    <h1>TodoMVC</h1>
    <form on:submit={addTodo}>
      <input
        class="new-todo"
        placeholder="What needs to be done?"
        autofocus
        bind:value={newTodo} />
    </form>
  </header>
  <!-- This section should be hidden by default and shown when there are todos -->
  <section class="main">
    <ul class="todo-list">
      <!-- These are here just to show the structure of the list items -->
      <!-- List items should get the class `editing` when editing and `completed` when marked as completed -->
      {#each filteredTodos as todo}
        <li class={todo.completed ? 'completed' : ''}>
          <div class="view">
            <input
              class="toggle"
              type="checkbox"
              bind:checked={todo.completed} />
            <label>{todo.text}</label>
            <button class="destroy" on:click={() => deleteTodo(todo.id)} />
          </div>
        </li>
      {/each}
    </ul>
  </section>
  <!-- This footer should hidden by default and shown when there are todos -->
  <footer class="footer">
    <!-- This should be `0 items left` by default -->
    <span class="todo-count">
      <strong>{numActive}</strong>
      item left
    </span>
    <!-- Remove this if you don't implement routing -->
    <ul class="filters">
      {#each FILTER_TITLES as filterName}
        <li key={filterName}>
          <a
            href="#"
            class={filter === filterName ? 'selected' : ''}
            style="cursor: pointer"
            on:click={() => (filter = filterName)}>
            {filterName}
          </a>
        </li>
      {/each}
    </ul>
    <!-- Hidden if no completed items are left ↓ -->
    <button class="clear-completed" on:click={clearCompleted}>
      Clear completed
    </button>
  </footer>
</section>
