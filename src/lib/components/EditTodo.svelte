<script lang="ts">
  import type { Todo } from "$lib/types/Todo";
    import { createEventDispatcher } from "svelte";
  
    const dispatch = createEventDispatcher();

    export let todo: Todo;
  
    function addTodo() {
      fetch(`/api/todos/${todo.id}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(todo),
      });
  
      dispatch("goto:list_todos");
    }
  </script>
  
  <form on:submit|preventDefault={addTodo}>
    <h1>Edit Todo</h1>
    <label for="title">Title</label>
    <input
      type="text"
      id="title"
      bind:value={todo.title}
      placeholder="The todo title..."
      required
    />
    <label for="dueDate">Due Date</label>
    <input type="date" id="dueDate" bind:value={todo.dueDate} required />
    <div class="actions">
      <button type="submit">Save</button>
      <button type="button" class="secondary" on:click={() => dispatch("goto:list_todos")}>Cancel</button>
    </div>
  </form>
  
  <style>
    form {
      align-self: center;
    }
  
    .actions {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: var(--pico-spacing);
    }
  
    button[type="submit"] {
      order: 2;
    }
  
    button[type="button"] {
      order: 1;
    }
  </style>
  