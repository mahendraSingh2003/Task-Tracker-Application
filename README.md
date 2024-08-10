### Task Tracker Application Documentation

---

#### **Project Overview**
The Task Tracker application is a simple, web-based tool that allows users to manage their daily tasks efficiently. It is built using vanilla JavaScript, HTML, and CSS, focusing on basic CRUD (Create, Read, Update, Delete) operations. The application provides a user-friendly interface where users can add, view, edit, and delete tasks seamlessly.

---

#### **1. Project Setup**

1. **Create Project Directory:**
   - Create a new directory for your project and name it something meaningful, e.g., `task-tracker`.

2. **Set Up Basic Files:**
   - Inside the project directory, create three essential files:
     - `index.html`: This will contain the HTML structure of the application.
     - `style.css`: This file will hold the CSS to style the application.
     - `script.js`: This file will contain the JavaScript code to handle the functionality.

---

#### **2. Design the User Interface (UI)**

The UI is designed to be simple and intuitive, allowing users to manage their tasks easily. The UI consists of the following elements:

1. **HTML Structure (`index.html`):**
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Task Tracker Application</title>
       <link rel="stylesheet" href="style.css">
   </head>
   <body>
      <div class="container">
        <h1>Task Tracker App.</h1>
        <div class="input-container">
            <input type="text" id="task-input" placeholder="Add a new task...">
            <button id="add-task-button">Add</button>
        </div>
        <ul id="task-list"></ul>
        <div class="filter-buttons">
            <button class="filter" data-filter="all">All</button>
            <button class="filter" data-filter="active">Active</button>
            <button class="filter" data-filter="completed">Completed</button>
        </div>
    </div>
    <script src="script.js"></script>
   </body>
   </html>
   ```

2. **CSS Styling (`style.css`):**
   ```css

   body {
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    }

    .container {
    background-color:#fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    width: 300px;
    text-align: center;
   }

   h1 {
    margin-bottom: 20px;
   }

   .input-container {
    display: flex;
    justify-content: space-between;
     margin-bottom: 20px;
     }

   #task-input {
    flex: 1;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
   }

   #add-task-button {
    padding: 10px;
    border: none;
    background-color: #28a745;
    color: white;
    border-radius: 4px;
    margin-left: 10px;
    cursor: pointer;
    }

   #task-list {
    list-style: none;
    padding: 0;
    margin: 0;
   }

   #task-list li {
    display: flex;
    justify-content: space-between;
    padding: 10px;
    border-bottom: 1px solid #ddd;
      }
   .task {
    flex: 1;
    text-align: left;
    }

    .task.completed {
    text-decoration: line-through;
    color: #888;
    }
 
   .delete-button {
    background: none;
    border: none;
    color: #e74c3c;
    cursor: pointer;
     }
     .update-button {
    background: none;
    border: none;
    color: #FFCC00;
    cursor: pointer;
       }


       .filter-buttons {
    margin-top: 20px;
    }

     .filter {
    padding: 5px 10px;
    border: none;
    cursor: pointer;
    background-color: #f1f1f1;
    margin: 0 5px;
    border-radius: 4px;
    }

   .filter.active {
    background-color: #007bff;
    color: white;
   }
   
   ```

---

#### **3. Implementing CRUD Operations**

1. **JavaScript Functionality (`script.js`):**
   ```javascript
   document.addEventListener('DOMContentLoaded', () => {
    const taskInput = document.getElementById('task-input');
    const addTaskButton = document.getElementById('add-task-button');
    const taskList = document.getElementById('task-list');
    const filterButtons = document.querySelectorAll('.filter');

    // Function to add a new task
    const addTask = () => {
        const taskText = taskInput.value.trim();
        if (taskText) {
            const li = document.createElement('li');
            li.innerHTML = `
                <span class="task">${taskText}</span>
                <button class="update-button">Update</button>
                <button class="delete-button">Delete</button>
            `;
            taskList.appendChild(li);
            taskInput.value = '';
            updateTaskList();
        }
    };

    // Function to update task list based on filter
    const updateTaskList = () => {
        const filter = document.querySelector('.filter.active').getAttribute('data-filter');
        const tasks = taskList.querySelectorAll('li');
        tasks.forEach(task => {
            const isCompleted = task.querySelector('.task').classList.contains('completed');
            switch (filter) {
                case 'all':
                    task.style.display = '';
                    break;
                case 'active':
                    task.style.display = isCompleted ? 'none' : '';
                    break;
                case 'completed':
                    task.style.display = isCompleted ? '' : 'none';
                    break;
            }
        });
    };

    // Event listener for adding tasks
    addTaskButton.addEventListener('click', addTask);
    taskInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') {
            addTask();
        }
    });

     // Function to update task text
     const updateTask = (taskItem) => {
        const taskText = taskItem.querySelector('.task').textContent;
        const newTaskText = prompt('Update the task', taskText);
        if (newTaskText !== null && newTaskText.trim() !== '') {
            taskItem.querySelector('.task').textContent = newTaskText.trim();
        }
    };
    

    // Event listener for marking tasks as completed and deleting tasks
    taskList.addEventListener('click', (e) => {
        if (e.target.classList.contains('task')) {
            e.target.classList.toggle('completed');
        } else if (e.target.classList.contains('delete-button')) {
            e.target.parentElement.remove();
        }else if(e.target.classList.contains('update-button')){
            updateTask(e.target.parentElement);
        }
        updateTaskList();
      });

    // Event listener for filtering tasks
    filterButtons.forEach(button => {
        button.addEventListener('click', () => {
            filterButtons.forEach(btn => btn.classList.remove('active'));
            button.classList.add('active');
            updateTaskList();
        });
       });
 
    // Initialize with all filter active
       filterButtons[0].classList.add('active');
    });
   ```

2. **Functionality Explanation:**
   - **Add Task:** The `addTask` function is triggered when the "Add Task" button is clicked. It creates a new list item (`li`) element with the task text and adds it to the task list.
   - **Display Tasks:** All tasks are displayed dynamically in the `ul` element with the ID `task-list`.
   - **Update Task:** The `editTask` function prompts the user to edit the selected task. If the new task text is valid, it updates the task.
   - **Delete Task:** The `deleteTask` function removes the selected task from the list.

---

#### **4. Testing the Application**

- **Add Task:** Test by entering various tasks in the input field and clicking the "Add Task" button. Verify that each task is added to the list.
- **Edit Task:** Test by clicking the "Edit" button on a task, modifying the task text, and confirming that the change is reflected in the list.
- **Delete Task:** Test by clicking the "Delete" button on a task and confirming that the task is removed from the list.

---

#### **5. Documentation and Setup Instructions**

**Setup Instructions:**

1. Clone or download the project repository.
2. Open the project directory.
3. Open `index.html` in your web browser.

**Functionalities Overview:**

- **Add Task:** Users can add a new task using the input field and "Add Task" button.
- **Display Tasks:** The application displays all tasks in a list format.
- **Edit Task:** Users can edit any existing task by clicking the "Edit" button.
- **Delete Task:** Users can delete any task by clicking the "Delete" button.

---

This documentation should provide a clear guide to setting up, using, and understanding the Task Tracker application.
