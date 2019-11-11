<script>
  import TodoItem from "./TodoItem.svelte";
  let newTodoTitle = "";
  let currentFilter = "all";
  let nextId = 4;
  let todos = [
    {
      id: 1,
      title: "auto wassen",
      completed: false
    },
    {
      id: 2,
      title: "vaatwas",
      completed: false
    },
    {
      id: 3,
      title: "boekentas maken",
      completed: false
    }
  ];
  function addTodo(event) {
    if (event.key === "Enter") {
      todos = [
        ...todos,
        {
          id: nextId,
          completed: false,
          title: newTodoTitle
        }
      ];
      nextId = nextId + 1;
      newTodoTitle = "";
    }
  }
  $: todosRemaining = filteredTodos.filter(todo => !todo.completed).length;
  $: filteredTodos =
    currentFilter === "all"
      ? todos
      : currentFilter === "completed"
      ? todos.filter(todo => todo.completed)
      : todos.filter(todo => !todo.completed);
  function checkAllTodos(event) {
    todos.forEach(todo => (todo.completed = event.target.checked));
    todos = todos;
  }
  function updateFilter(newFilter) {
    currentFilter = newFilter;
  }
  function clearCompleted() {
    todos = todos.filter(todo => !todo.completed);
  }
  function handleDeleteTodo(event) {
    todos = todos.filter(todo => todo.id !== event.detail.id);
  }
  function handleToggleComplete(event) {
    const todoIndex = todos.findIndex(todo => todo.id === event.detail.id);
    const updatedTodo = {
      ...todos[todoIndex],
      completed: !todos[todoIndex].completed
    };
    todos = [
      ...todos.slice(0, todoIndex),
      updatedTodo,
      ...todos.slice(todoIndex + 1)
    ];
  }
</script>

<style>
  * {
    font-family: "Kanit", sans-serif;
  }
  @import url("https://fonts.googleapis.com/css?family=Kanit&display=swap");
  .container {
    max-width: 800px;
    margin: 10px auto;
  }
  .todo-input {
    width: 100%;
    padding: 10px, 20px;
    font-size: 18px;
    margin-bottom: 20px;
    outline-color: rgb(0, 89, 255);
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
    margin-right: 15px;
  }
  h1 {
    font-family: "Kanit", sans-serif;
  }

  button {
    padding: 12px;
    font-size: 18px;

    background-color: white;
    appearance: none;
  }
  button:hover {
    background: rgb(46, 16, 216);
    color: white;
  }
  h1 {
    text-align: center;
  }
  button:focus {
    outline: none;
  }
  .active {
    background: rgb(0, 89, 255);
    color: white;
  }

  @media only screen and (max-width: 600px) {
    .remove-item {
      margin-right: 10px;
    }
    .todo-item-left {
      margin-right: 10px;
    }
    .todo-item-wisssen {
      margin-right: 10px;
    }
    button {
      margin-left: 10px;
      padding: 10px;
    }
  }
</style>

<div class="container">
  <h1>Todos</h1>
  <input
    type="text"
    class="todo-input"
    placeholder="Enter Todo item..."
    bind:value={newTodoTitle}
    on:keydown={addTodo} />

  {#each filteredTodos as todo}
    <div class="todo-item">
      <TodoItem
        {...todo}
        on:deleteTodo={handleDeleteTodo}
        on:toggleComplete={handleToggleComplete} />
    </div>
  {/each}

  <div class="inner-container">
    <div>
      <label>
        <input
          class="inner-container-input"
          type="checkbox"
          on:change={checkAllTodos} />
        Vink Alles Aan
      </label>
    </div>
    <div class="todo-item-left">{todosRemaining} items left</div>
  </div>

  <div class="inner-container">
    <div>
      <button
        class="todo-alles"
        on:click={() => updateFilter('all')}
        class:active={currentFilter === 'all'}>
        Alles
      </button>
      <button
        on:click={() => updateFilter('active')}
        class:active={currentFilter === 'active'}>
        Actief
      </button>
      <button
        on:click={() => updateFilter('completed')}
        class:active={currentFilter === 'completed'}>
        Voltooid
      </button>
    </div>
    <dir>
      <button class="todo-item-wisssen" on:click={clearCompleted}>
        Wissen Voltooid
      </button>
    </dir>
  </div>

</div>
