<script>
  export let todos;

  $: totalTodos = todos.length;
  $: completedTodos = todos.filter((todo) => todo.completed).length;
  let headingEl;
  export const onFocus = () => {
    headingEl.focus();
  };
</script>

<h2 id="list-heading" bind:this={headingEl} tabindex="-1">
  {completedTodos} out of {totalTodos} items completed
</h2>
