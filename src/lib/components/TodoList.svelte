<script lang="ts">
  import type { Todo } from "$lib/types/Todo";
  import { PenBox, Plus, Trash2 } from "lucide-svelte";
  import { DateTime } from "luxon";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let todosPromise: Promise<Todo[]> = fetch("/api/todos").then((res) =>
    res.json()
  );

  function toggleTodo(todo: Todo) {
    fetch(`/api/todos/${todo.id}`, {
      method: "PATCH",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ done: todo.done }),
    });
  }

  function deleteTodo(todo: Todo) {
    fetch(`/api/todos/${todo.id}`, {
      method: "DELETE",
    }).then(() => {
      todosPromise = fetch("/api/todos").then((res) => res.json());
    });
  }
</script>

<div>
  <div class="header">
    <h1>Your Todos</h1>
    <button class="outline secondary" on:click={() => dispatch("goto:add_todo")}
      ><Plus /></button
    >
  </div>

  {#await todosPromise}
    <div aria-busy="true"></div>
  {:then todos}
    <ul>
      {#each todos as todo (todo.id)}
        {@const overdue =
          DateTime.now().startOf("day") > DateTime.fromISO(todo.dueDate)}
        {@const due = DateTime.now().hasSame(
          DateTime.fromISO(todo.dueDate),
          "day"
        )}
        <li>
          <label for="todo-{todo.id}" class:overdue class:due>
            <input
              type="checkbox"
              id="todo-{todo.id}"
              bind:checked={todo.done}
              on:change={() => toggleTodo(todo)}
            />
            {todo.title}
          </label>
          <span class="actions">
            <button
              class="unstyled"
              on:click={() => dispatch("goto:edit_todo", { ...todo })}
              ><PenBox size="18" /></button
            >
            <button class="unstyled" on:click={() => deleteTodo(todo)}
              ><Trash2 size="18" /></button
            >
          </span>
        </li>
      {:else}
        <p>No todos yet.</p>
      {/each}
    </ul>
  {:catch error}
    <p class="error">{error.message}</p>
  {/await}
</div>

<style>
  ul {
    list-style: none;
    padding: 0;
  }

  li {
    list-style: none;
    padding-top: var(--pico-spacing);
    padding-bottom: var(--pico-spacing);
    border-bottom: solid thin var(--pico-muted-border-color);
    margin: 0;
    display: flex;
    justify-content: space-between;
    gap: var(--pico-spacing);
    align-items: center;
  }

  li:first-child {
    border-top: solid thin var(--pico-muted-border-color);
  }

  label {
    margin: 0;
    width: 100%;
  }

  .error {
    color: var(--pico-color-red-500);
  }

  .due {
    color: var(--pico-color-pumpkin-500);
  }

  .overdue {
    color: var(--pico-color-orange-500);
  }

  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: calc(var(--pico-spacing) * 2);
  }

  .header h1 {
    margin: 0;
  }

  .unstyled {
    padding: 0;
    border: none;
    background: none;
    box-shadow: none;
    color: var(--pico-color);
  }

  .actions {
    display: flex;
    gap: var(--pico-spacing);
  }
</style>
