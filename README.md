# ToDo App — Vue 3 + TypeScript

A Todo application built to practice **modern Vue development with TypeScript**, focusing on clean architecture and reactivity.

---

## Overview

The app allows users to manage daily tasks with a responsive and reactive interface.
Data is persisted in `localStorage`, so tasks remain available between sessions.

---

## Features

* Create, edit, and delete tasks
* Mark tasks as completed
* Deadline support with overdue detection
* Persistent state via `localStorage`
* Reactive UI with instant updates
* Basic validation (e.g. empty input handling)

---

## Architecture

```
src/
  components/
    TodoForm.vue      # handles task creation
    TodoItem.vue      # single task logic
    TodoList.vue      # renders list of tasks
  types/
    todo.ts           # TypeScript interfaces
  App.vue             # root state & logic
  main.ts             # app entry
```

Key idea: keep **logic close to where it’s used**, while avoiding unnecessary complexity.

---

## Setup

```bash
git clone https://github.com/skyzerx777/toDo-vue.git
cd toDo-vue
npm install
npm run dev
```

---

## Demo

<img width="1800" height="885" alt="Screenshot_chrome_2026-05-03_13 53 49" src="https://github.com/user-attachments/assets/fd9ce085-d97e-4436-b41c-ccaa2d62d9f3" />

---

## Author

GitHub: https://github.com/skyzerx777  
LinkedIn: https://www.linkedin.com/in/yevhenii-hrebenshchykov-10b5b1264/
