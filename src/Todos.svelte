<script>
  import TodoItem from "./TodoItem.svelte";
  let todos = [
    {
      id: 1,
      title: "My first todo",
      completed: false,
    },
    {
      id: 2,
      title: "My second todo",
      completed: false,
    },
    {
      id: 3,
      title: "My third todo",
      completed: false,
    },
  ];

  let newTodoTitle = "";
  let currentFilter = "all";
  let nextId = 4;

  $: todosRemaining = filteredTodos.filter((todo) => !todo.completed).length;
  $: filteredTodos =
    currentFilter === "all"
      ? todos
      : currentFilter === "completed"
      ? todos.filter((todo) => todo.completed)
      : todos.filter((todo) => !todo.completed);

  function addTodo(event) {
    if (event.key === "Enter") {
      todos = [
        ...todos,
        {
          id: nextId,
          completed: false,
          title: newTodoTitle,
        },
      ];
      nextId = nextId + 1;
      newTodoTitle = "";
    }
  }

  function checkAllTodos(event) {
    todos.forEach((todo) => (todo.completed = event.target.checked));
    todos = todos;
  }

  function updateFilter(newFilter) {
    currentFilter = newFilter;
  }

  function clearCompleted() {
    todos = todos.filter((todo) => !todo.completed);
  }

  function handleDeleteTodo(event) {
    todos = todos.filter((todo) => todo.id !== event.detail.id);
  }

  function handleToggleComplete(event) {
    const todoIndex = todos.findIndex((todo) => todo.id === event.detail.id);
    const updatedTodo = {
      ...todos[todoIndex],
      completed: !todos[todoIndex].completed,
    };
    todos = [
      ...todos.slice(0, todoIndex),
      updatedTodo,
      ...todos.slice(todoIndex + 1),
    ];
  }
</script>

<div class="container">
  <a href="https://codingthesmartway.com" target="_blank">
    <img src={"/img/CTSWLogo.png"} alt="svelte logo" class="logo" />
  </a>
  <h2>Svelte Todo App</h2>
  <input
    class="todo-input"
    type="text"
    placeholder="Insert todo item ..."
    bind:value={newTodoTitle}
    on:keydown={addTodo}
  />

  {#each filteredTodos as todo}
    <div class="todo-item">
      <TodoItem
        {...todo}
        on:deleteTodo={handleDeleteTodo}
        on:toggleComplete={handleToggleComplete}
      />
    </div>
  {/each}

  <div class="inner-container">
    <div>
      <label>
        <input
          class="inner-container-input"
          type="checkbox"
          on:change={checkAllTodos}
        />
        Check All
      </label>
    </div>
    <div>{todosRemaining} items left</div>
  </div>

  <div class="inner-container">
    <div>
      <button
        class:active={currentFilter === "all"}
        on:click={() => updateFilter("all")}
      >
        All
      </button>
      <button
        class:active={currentFilter === "active"}
        on:click={() => updateFilter("active")}
      >
        Active
      </button>
      <button
        class:active={currentFilter === "completed"}
        on:click={() => updateFilter("completed")}
      >
        Completed
      </button>
    </div>
    <div>
      <button on:click={clearCompleted}> Clear Completed </button>
    </div>
  </div>
</div>

<style>
  .container {
    max-width: 800px;
    margin: 10px auto;
  }
  .logo {
    display: block;
    margin: 20px auto;
    width: 50%;
  }
  .todo-input {
    width: 100%;
    padding: 10px, 20px;
    font-size: 18px;
    margin-bottom: 20px;
  }
  .inner-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 15px;
    margin-bottom: 13px;
  }
  .inner-container-input {
    margin-right: 12px;
  }
  button {
    font-size: 14px;
    background-color: white;
    appearance: none;
  }
  button:hover {
    background: lightseagreen;
  }
  button:focus {
    outline: none;
  }
  .active {
    background: lightseagreen;
  }
</style>
