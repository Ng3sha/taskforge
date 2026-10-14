# TaskForge

A task management dashboard built with vanilla HTML, CSS, and JavaScript. TaskForge lets you create, edit, search, filter, sort, and delete tasks entirely through DOM manipulation, with no frameworks or libraries.

## Features

- Add, edit, and delete tasks (with form validation and a delete confirmation modal)
- Mark tasks as completed or pending
- Search by title, description, or category (debounced)
- Combined filters: status, priority, and category
- Sorting: newest, oldest, A → Z, Z → A, priority, and due date
- Live statistics: total, completed, pending, and overdue
- Bulk actions: select all, delete selected, mark completed or pending
- Task details modal
- Overdue detection with distinct styling
- Empty state and "No tasks found" messages
- Unsaved-changes warning when editing
- Dark mode (preference saved)
- 10+ sample tasks on first load
- Keyboard shortcuts:
  - `N`: new task form
  - `/`: focus search
  - `Esc`: close modal
  - `Ctrl + Enter`: submit form

## Project Structure

```
index.html   # page structure
style.css    # styles, themes, priority/status classes
app.js       # application logic
README.md
```

## Running the Project

1. Clone the repository.
2. Open `index.html` in a browser. No build step or server is needed.

## Major DOM Methods Used

| Category | Methods |
| --- | --- |
| Selecting | `getElementById()`, `querySelector()`, `querySelectorAll()`, `closest()` |
| Creating and inserting | `createElement()`, `createDocumentFragment()`, `append()`, `appendChild()`, `prepend()`, `insertAdjacentHTML()`, `cloneNode()` |
| Removing and replacing | `remove()`, `removeChild()`, `replaceWith()` |
| Content | `textContent`, `innerHTML` |
| Attributes and classes | `classList`, `setAttribute()`, `getAttribute()`, `removeAttribute()`, `dataset` |
| Traversal | `parentElement`, `children`, `firstElementChild`, `lastElementChild`, `nextElementSibling`, `previousElementSibling` |
| Events and forms | `addEventListener()`, `preventDefault()`, `target`, `currentTarget`, `FormData` |
| Storage | `localStorage`, `JSON.stringify()`, `JSON.parse()` |

## Event Delegation

Task cards are generated dynamically, so listeners are not attached to each button. Instead, a single click listener is attached to the task list container. When a click happens, `event.target.closest()` finds the button or control that was clicked, and its action is read from a `data-action` attribute (`edit`, `delete`, `complete`, `select`, `view`). The task is then located with `closest('.task')`, and its ID is read from `data-id`. This keeps the code efficient and works for tasks added at any time.

## Task Management Logic

- Each task is an object: `id`, `title`, `description`, `category`, `priority`, `dueDate`, `createdAt`, and `completed`.
- **Create:** the form is read with `FormData`, validated, saved, and rendered. The statistics are updated and the form is cleared.
- **Edit:** the task's data is loaded into the form, and the task is updated on submit. The app tracks the original values to detect unsaved changes.
- **Delete:** a confirmation modal opens first, and the task is removed only after the user confirms.
- **Overdue:** a task is overdue if its due date has passed and it is not completed.
- **Display pipeline:** every render runs the same steps: search, then filters, then sort, then render. Because of this, all filters work together.
- **CSS classes:** `classList` adds or removes classes for status (`completed`, `pending`, `overdue`) and priority (`low`, `medium`, `high`, `critical`).

## localStorage Implementation

| Key | Purpose |
| --- | --- |
| `tasks` | the array of tasks, saved with `JSON.stringify()` |
| `darkMode` | the saved theme preference |

On startup, the app reads the saved tasks with `JSON.parse()` and renders them. If nothing is stored (first visit), it loads the sample tasks and saves them. Every add, edit, delete, or status change saves the updated array back to `localStorage`.