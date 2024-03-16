# Full Stack Development with Svelte & SvelteKit – Part I

This repository contains the project code for Part I of the workshop. On this readme, you can find all information
necessary to work the hands-on exercise of Part I.

## Resources

- [Svelte Documentation](https://svelte.dev/docs/introduction)
- [Available Lucide Icons](https://lucide.dev/icons/)
- [Pico (CSS) Documentation](https://picocss.com/docs)

## Getting Started

First things first, you need to clone this repository to your local machine. After that, you first can install
dependencies and then initialize the database. After that it should be possible to run the dev server,

If you prefer to work in a sandboxed browser environment, you can run open the repository in StackBlitz, instead.
Everything should work the same.

<a href="https://stackblitz.com/github/nilsroehrig/full-stack-svelte-part-1">
  <img
    alt="Open in StackBlitz"
    src="https://developer.stackblitz.com/img/open_in_stackblitz.svg"
  />
</a>

```bash
# clone the repo
git clone https://github.com/nilsroehrig/full-stack-svelte-part-1.git

# in repository folder, checkout exercise starting point
git checkout exercise -b dev_branch

# install deps
npm ci

# initialize db
node init.js

# run dev server
npm run dev
```

The dev server is available at [localhost:5173](http://localhost:5173).

## Hands-On Exercise

1. When toggling a todo, persist the change. You can use a PATCH request to /api/todos/{todoId} for this.
2. Add a new view for adding a todo item. You can use a POST requst to /api/todos for this.
3. Connect to the new view from the todo list. Make sure, that there is a way back.
4. Add a new view for editing existing todos. You can use a PUT request to /api/todos/{todoId} for this.
5. Connect to the new view from the todo list. Make sure that there is a way back.
6. Add an option to delete a todo. you can use a DELETE request to /api/todos/{todoId} for this.
