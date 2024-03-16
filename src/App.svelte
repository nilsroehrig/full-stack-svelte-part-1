<script lang="ts">
  import { ListTodo } from "lucide-svelte";
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
      <p class="centered">Content goes here!</p>
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

  .centered {
    align-self: center;
    justify-self: center;
  }
</style>
