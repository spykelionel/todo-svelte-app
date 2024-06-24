<script>
  import FilterButton from "./FilterButton.svelte";
  import MoreActions from "./MoreActions.svelte";
  import Todo from "./Todo.svelte";

  export let todos = [];
  $: totalTodos = todos.length;
  $: completedTodos = todos.filter((todo) => todo.completed).length;

  function removeTodo(e) {
    todos = todos.filter((t) => t.id !== e.detail.todo.id);
  }

  let newTodo = "";
  function addTodo() {
    if (newTodo.length < 1) {
      alert("Field can not be empty");
      return;
    }
    const value = { completed: false, id: todos.length + 1, name: newTodo };
    todos = [...todos, value];
    newTodo = "";
  }

  let filter = "all";
  const filterTodos = (filter, todos) =>
    filter === "active"
      ? todos.filter((t) => !t.completed)
      : filter === "completed"
        ? todos.filter((t) => t.completed)
        : todos;

  const markCompleted = (e) =>
    (e.detail.todo.completed = !e.detail.todo.completed);

  function updateTodo(e) {
    const { todo } = e.detail;
    const i = todos.findIndex((t) => t.id === todo.id);
    todos[i] = { ...todos[i], ...todo };
  }

  function checkAllTodos(e) {
    const { completed } = e.detail;
    todos = todos.map((t) => ({ ...t, completed }));
  }

  function removeCompletedTodos() {
    todos = todos.filter((t) => !t.completed);
  }
</script>

<!-- Todos.svelte -->
<div class="todoapp stack-large">
  <!-- NewTodo -->
  <form on:submit|preventDefault={addTodo}>
    <h2 class="label-wrapper">
      <label for="todo-0" class="label__lg"> What needs to be done? </label>
    </h2>
    <input
      bind:value={newTodo}
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
  <FilterButton bind:filter />

  <!-- TodosStatus -->
  <h2 id="list-heading">
    {completedTodos} out of {totalTodos} items completed
  </h2>
  <!-- Todos -->
  <!-- svelte-ignore a11y-no-redundant-roles -->
  <!-- To-dos -->
  <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
    {#each filterTodos(filter, todos) as todo (todo.id)}
      <li class="todo">
        <Todo
          {todo}
          on:update={updateTodo}
          on:complete={markCompleted}
          on:remove={removeTodo}
        />
      </li>
    {:else}
      <li>Nothing to do here!</li>
    {/each}
  </ul>

  <hr />

  <!-- MoreActions -->
  <MoreActions
    on:checkAll={checkAllTodos}
    on:removeCompleted={removeCompletedTodos}
    {todos}
  />
</div>
