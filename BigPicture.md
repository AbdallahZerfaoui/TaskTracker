
# Task Tracker

#### *Build a CLI app to track your tasks and manage your to-do list.*


This plan focuses on breaking down the project into manageable steps, encouraging learning along the way, without giving you the specific code.

**Goal:** Build a command-line task tracker in Python using only built-in modules, storing data in `tasks.json`.

**Core Concepts to Practice:**

*   Reading command-line arguments (`sys.argv`).
*   File I/O (reading/writing files).
*   JSON data handling (`json` module).
*   Data structures (lists and dictionaries).
*   Basic error handling (`try...except`).
*   Working with dates/times (`datetime` module).
*   Structuring a simple application (functions, main execution block).

---

**Big Picture Plan: Building the Task Tracker CLI**

**Phase 1: Foundation - Setup and Data Handling**

1.  **Project Setup:**
    *   Create your project directory (e.g., `task_tracker_cli`).
    *   Inside, create your main Python script file (e.g., `task_cli.py`).
    *   Initialize Git (`git init`) for version control (good practice!).
    *   Define the name for your data file (e.g., `tasks.json`).

2.  **Data Storage Logic:**
    *   **Goal:** Create functions to reliably load tasks from `tasks.json` and save tasks back to it.
    *   **Function 1: `load_tasks()`**
        *   Check if `tasks.json` exists (`os.path.exists`).
        *   If it exists:
            *   Open and read the file.
            *   Use the `json` module to parse the content into a Python list of dictionaries.
            *   Handle potential errors: What if the file exists but is empty or contains invalid JSON? Return an empty list `[]` in these cases.
        *   If it doesn't exist: Return an empty list `[]`.
    *   **Function 2: `save_tasks(tasks_list)`**
        *   Takes a Python list of task dictionaries as input.
        *   Opens `tasks.json` in write mode (`'w'`) - this will overwrite the file or create it if it doesn't exist.
        *   Use the `json` module to convert the Python list into a JSON string and write it to the file. Use indentation (`json.dump(..., indent=4)`) to make the JSON file human-readable.
        *   Handle potential file writing errors.

**Phase 2: Core Feature - Adding Tasks**

3.  **Implement `add` Command:**
    *   **Goal:** Allow users to add a new task with a description.
    *   **Task Structure:** Define what a task dictionary looks like: `{ 'id': ..., 'description': ..., 'status': 'todo', 'createdAt': ..., 'updatedAt': ... }`.
    *   **Function: `add_task(description)`**
        *   Load existing tasks using `load_tasks()`.
        *   Determine the next unique `id`. A simple way is to find the highest existing ID and add 1 (or start at 1 if the list is empty).
        *   Get the current timestamp using the `datetime` module for `createdAt` and `updatedAt`. Format it consistently (e.g., ISO format: `datetime.datetime.now().isoformat()`).
        *   Create the new task dictionary.
        *   Append the new task to the list loaded earlier.
        *   Save the updated list using `save_tasks()`.
        *   Print a confirmation message to the user (e.g., "Task added successfully (ID: X)").

**Phase 3: Viewing Tasks - Listing**

4.  **Implement `list` Command (Basic):**
    *   **Goal:** Allow users to see all their tasks.
    *   **Function: `list_tasks()`** (or maybe a more flexible `display_tasks(tasks_list)`)
        *   Load tasks using `load_tasks()`.
        *   If the list is empty, print a message like "No tasks found."
        *   If tasks exist, iterate through the list and print each task's details (ID, Status, Description) in a readable format.

**Phase 4: Command Line Integration (Partial)**

5.  **Basic Argument Parsing:**
    *   **Goal:** Make the script respond to command-line arguments.
    *   Import the `sys` module.
    *   In the main part of your script (e.g., under `if __name__ == "__main__":`), get the command-line arguments using `sys.argv`. Remember `sys.argv[0]` is the script name itself.
    *   Check if arguments were provided.
    *   Identify the *command* (e.g., `add`, `list`) which should be `sys.argv[1]`.
    *   Identify the *arguments* for that command (e.g., the description for `add`, which would be `sys.argv[2]`).
    *   Use `if/elif/else` statements to call the appropriate function based on the command.
    *   **Test:** Run `python task_cli.py add "My first task"` and `python task_cli.py list`. Check `tasks.json`.

**Phase 5: Expanding Functionality - Modifying Tasks**

6.  **Implement `update` Command:**
    *   **Goal:** Allow users to change the description of an existing task.
    *   **Function: `update_task(task_id, new_description)`**
        *   Load tasks.
        *   Find the task with the matching `task_id`. Handle the case where the ID doesn't exist. Remember to convert the ID from the command line (string) to an integer for comparison.
        *   If found, update its `description`.
        *   Update its `updatedAt` timestamp.
        *   Save tasks.
        *   Print confirmation or error message.
    *   **Update CLI Parsing:** Add an `elif` block for the `update` command, ensuring it receives both the `id` and the `new_description` arguments. Handle potential errors (missing arguments, invalid ID format).

7.  **Implement `delete` Command:**
    *   **Goal:** Allow users to remove a task.
    *   **Function: `delete_task(task_id)`**
        *   Load tasks.
        *   Find the task with the matching `task_id`. Handle "not found". Convert ID to an integer.
        *   If found, remove it from the list.
        *   Save the modified list.
        *   Print confirmation or error message.
    *   **Update CLI Parsing:** Add an `elif` block for `delete`, handling arguments and errors.

8.  **Implement `mark-in-progress` and `mark-done` Commands:**
    *   **Goal:** Change the status of a task.
    *   **Consider:** You could have one function `update_task_status(task_id, new_status)` or two separate ones. A single function might be slightly cleaner.
    *   **Function: `update_task_status(task_id, new_status)`**
        *   Load tasks.
        *   Find task by `task_id` (convert to int, handle not found).
        *   Validate `new_status` (should be 'in-progress' or 'done').
        *   If found and the status is valid, update the `status` field.
        *   Update the `updatedAt` timestamp.
        *   Save tasks.
        *   Print confirmation or error message.
    *   **Update CLI Parsing:** Add `elif` blocks for `mark-in-progress` (calling `update_task_status` with 'in-progress') and `mark-done` (calling with 'done'). Handle arguments and errors.

**Phase 6: Enhancing Listing and Final Touches**

9.  **Implement Filtered Listing:**
    *   **Goal:** Allow `list` command to filter by status (`todo`, `in-progress`, `done`).
    *   **Modify `list_tasks` (or create a new filtered version):**
        *   Add an optional argument `filter_status=None`.
        *   Inside the function, when iterating through tasks, check if `filter_status` is provided.
        *   If it is, only print tasks whose `status` matches `filter_status`.
        *   If `filter_status` is `None`, print all tasks (as before).
    *   **Update CLI Parsing for `list`:** Check if `sys.argv[2]` exists. If it does, use it as the `filter_status` when calling your list function. Validate the filter status if provided.

10. **Error Handling and Edge Cases:**
    *   Go through your code (especially the CLI parsing part).
    *   Wrap potentially problematic operations (file I/O, list index access, `int()` conversion) in `try...except` blocks.
    *   Catch specific exceptions (`FileNotFoundError`, `IOError`, `json.JSONDecodeError`, `IndexError`, `ValueError`).
    *   Print user-friendly error messages (e.g., "Error: Task ID must be a number.", "Error: Task with ID X not found.", "Error: Invalid command.").
    *   Consider what happens if the user provides too many or too few arguments for a command.

11. **Refinement and Documentation:**
    *   Review your code for clarity and consistency. Add comments where the logic isn't obvious.
    *   Make sure the output messages are clear and helpful.
    *   Create a `README.md` file explaining:
        *   What the project does.
        *   How to run it (e.g., `python task_cli.py <command> [arguments]`).
        *   List all available commands and their arguments with examples (like the ones provided in the prompt).

---

**Self-Correction/Refinement during the process:**

*   **ID Generation:** Is max ID + 1 robust enough? For this simple project, yes. For a real app, you might use UUIDs. Stick to the simple approach for now.
*   **Function Structure:** Should `load_tasks` and `save_tasks` be called inside every other function, or should the main CLI logic load once, pass the list to the function, and save once? Calling them inside each action function is simpler to start with, although slightly less efficient. Stick with the simpler approach first.
*   **Error Messages:** Are they informative? Do they tell the user *what* went wrong?

By following these phases, you build the application incrementally, testing each piece as you go. Good luck, and happy coding!
