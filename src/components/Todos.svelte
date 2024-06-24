<script>
  import FilterButton from "./FilterButton.svelte";
  import MoreActions from "./MoreActions.svelte";
  import NewTodo from "./NewTodo.svelte";
  import Todo from "./Todo.svelte";
  import TodosStatus from "./TodosStatus.svelte";

  export let todos = [];

  function removeTodo(e) {
    todos = todos.filter((t) => t.id !== e.detail.todo.id);
  }

  function addTodo(e) {
    const { name } = e.detail;
    const value = { completed: false, id: todos.length + 1, name };
    todos = [...todos, value];
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
  <NewTodo autofocus on:addTodo={addTodo} />
  <!-- Filter -->
  <FilterButton bind:filter />

  <!-- TodosStatus -->

  <TodosStatus {todos} />

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
