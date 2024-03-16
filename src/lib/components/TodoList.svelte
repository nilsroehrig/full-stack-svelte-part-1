<script lang="ts">
  import type { Todo } from "$lib/types/Todo";
  import { DateTime } from "luxon";

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
</script>

<div>
  <h1>Your Todos</h1>

  {#await todosPromise}
    <div aria-busy="true"></div>
  {:then todos}
    <ul>
      {#each todos as todo (todo.id)}
        {@const overdue =
          DateTime.now().startOf("day") > DateTime.fromISO(todo.dueDate)}
        {@const due =
          DateTime.now().hasSame(DateTime.fromISO(todo.dueDate), "day")}
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
</style>
