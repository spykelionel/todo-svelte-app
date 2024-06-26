<script>
  import { createEventDispatcher } from "svelte";
  import { focusOnInit, selectOnFocus } from "../actions";

  export let todo;
  const dispatch = createEventDispatcher();
  let editing = false; // track editing mode
  let name = todo.name; // hold the name of the to-do being edited
  let nameEl;
  let editButtonPressed = false; // track if edit button has been pressed, to give focus to it after cancel or save

  function update(updatedTodo) {
    todo = { ...todo, ...updatedTodo }; // applies modifications to todo
    dispatch("update", { todo }); // emit update event
  }

  function onCancel() {
    name = todo.name; // restores name to its initial value and
    editing = false; // and exit editing mode
  }

  function onSave() {
    update({ name }); // updates todo name
    editing = false; // and exit editing mode
  }

  function onRemove() {
    dispatch("remove", { todo }); // emit remove event
  }

  async function onEdit() {
    editing = true; // enter editing mode
    editButtonPressed = true;
  }

  const focusEditButton = (node) => editButtonPressed && node.focus();

  function onToggle() {
    update({ completed: !todo.completed }); // updates todo status
  }
</script>

<div class="stack-small">
  {#if editing}
    <form
      on:submit|preventDefault={onSave}
      class="stack-small"
      on:keydown={(e) => e.key === "Escape" && onCancel()}
    >
      <div class="form-group">
        <label for="todo-{todo.id}" class="todo-label"
          >New name for '{todo.name}'</label
        >
        <input
          bind:value={name}
          bind:this={nameEl}
          use:selectOnFocus
          use:focusOnInit
          type="text"
          id="todo-{todo.id}"
          autoComplete="off"
          class="todo-text"
        />
      </div>
      <div class="btn-group">
        <button class="btn todo-cancel" on:click={onCancel} type="button">
          Cancel<span class="visually-hidden">renaming {todo.name}</span>
        </button>
        <button
          class="btn btn__primary todo-edit"
          type="submit"
          disabled={!name}
        >
          Save<span class="visually-hidden">new name for {todo.name}</span>
        </button>
      </div>
    </form>
  {:else}
    <div class="c-cb">
      <input
        type="checkbox"
        id="todo-{todo.id}"
        on:click={onToggle}
        checked={todo.completed}
      />
      <label for="todo-{todo.id}" class="todo-label">{todo.name}</label>
    </div>
    <div class="btn-group">
      <button type="button" class="btn" on:click={onEdit} use:focusEditButton>
        Edit <span class="visually-hidden">{todo.name}</span>
      </button>
      <button type="button" class="btn btn__danger" on:click={onRemove}>
        Delete <span class="visually-hidden">{todo.name}</span>
      </button>
    </div>
  {/if}
</div>
