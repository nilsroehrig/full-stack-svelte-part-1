<script lang="ts">
  import AddTodo from "$lib/components/AddTodo.svelte";
  import EditTodo from "$lib/components/EditTodo.svelte";
  import TodoList from "$lib/components/TodoList.svelte";
  import type { Todo } from "$lib/types/Todo";
  import { ListTodo } from "lucide-svelte";

  let view: "list_todos" | "add_todo" | "edit_todo" = "list_todos";
  let selectedTodo: Todo;

  function editTodo(event: CustomEvent<Todo>) {
    view = "edit_todo";
    selectedTodo = event.detail;
  }
</script>

<div class="wrapper">
  <header>
    <nav class="container">
      <ul>
        <li><strong><ListTodo /> Svelte TodoList</strong></li>
      </ul>
    </nav>
  </header>

  <main class="container">
    {#if view === "list_todos"}
      <TodoList on:goto:add_todo={() => view = "add_todo"} on:goto:edit_todo={editTodo}/>
    {:else if view === "add_todo"}
      <AddTodo on:goto:list_todos={() => view = "list_todos"} />
    {:else if view === "edit_todo"}
      <EditTodo todo={selectedTodo} on:goto:list_todos={() => view = "list_todos"} />
    {:else}
      <p>Unknown view: {view}</p>
    {/if}
  </main>

  <footer>
    <div class="container">&copy; 2024 Svelte TodoList</div>
  </footer>
</div>

<style>
  .wrapper {
    display: grid;
    grid-template-rows: min-content auto min-content;
    height: 100vh;
  }

  header {
    border-bottom: solid thin var(--pico-muted-border-color);
  }

  nav {
    display: flex;
    justify-content: center;
  }

  main {
    display: grid;
    padding-top: var(--pico-spacing);
    padding-bottom: var(--pico-spacing);
  }

  footer {
    font-size: 0.875em;
    padding-top: var(--pico-spacing);
    padding-bottom: var(--pico-spacing);
    text-align: center;
    border-top: solid thin var(--pico-muted-border-color);
  }
</style>
