<script>
  import { createEventDispatcher } from "svelte";
  import { fly } from "svelte/transition";
  export let id;
  export let title;
  export let completed;
  const dispatch = createEventDispatcher();
  function deleteTodo() {
    dispatch("deleteTodo", {
      id: id
    });
  }
  function toggleComplete() {
    dispatch("toggleComplete", {
      id: id
    });
  }
</script>

<style>
  * {
    font-family: "Kanit", sans-serif;
  }
  @import url("https://fonts.googleapis.com/css?family=Kanit&display=swap");
  .todo-item {
    margin-bottom: 15px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    animation-duration: 0.3s;
  }
  .remove-item {
    cursor: pointer;
    margin-left: 15px;
  }
  .remove-item:hover {
    color: rgb(0, 228, 11);
  }
  .todo-item-left {
    display: flex;
    align-items: center;
  }
  .todo-item-label {
    border: 1px solid white;
    margin-left: 12px;
  }
  .completed {
    text-decoration: line-through;
    color: rgb(0, 238, 11);
  }
  @media only screen and (max-width: 600px) {
    .remove-item {
      margin-right: 10px;
    }
    .todo-item-label {
      margin-left: 10px;
    }
  }
</style>

<div class="todo-item">
  <div class="todo-item-left" transition:fly={{ y: 20, duration: 300 }}>
    <input
      type="checkbox"
      bind:checked={completed}
      on:change={toggleComplete} />
    <div class="todo-item-label" class:completed>{title}</div>
  </div>
  <div class="remove-item" on:click={deleteTodo}>x</div>
</div>
