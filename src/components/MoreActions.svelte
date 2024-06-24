<script>
  import { createEventDispatcher } from "svelte";
  import { alert } from "../store.js";

  const dispatch = createEventDispatcher();
  export let todos;
  let completed = true;

  const checkAll = () => {
    dispatch("checkAll", { completed });
    completed = !completed;
    $alert = "Todo has been toggled";
  };

  const removeCompleted = () => {
    dispatch("removeCompleted");
    $alert = "All completed todos removed";
  };
  $: completedTodos = todos.filter((t) => t.completed).length;
</script>

<div class="btn-group">
  <button
    disabled={todos.length === 0}
    type="button"
    class="btn btn__primary"
    on:click={checkAll}>{completed ? "Check" : "Uncheck"} all</button
  >
  <button
    disabled={completedTodos === 0}
    type="button"
    class="btn btn__primary"
    on:click={removeCompleted}>Remove completed</button
  >
</div>
