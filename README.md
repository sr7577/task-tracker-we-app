# task-tracker-we-app
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Task Tracker App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f5f5;
      padding: 40px;
    }

    .container {
      max-width: 500px;
      margin: auto;
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }

    h2 {
      text-align: center;
      margin-bottom: 20px;
    }

    input[type="text"] {
      width: 80%;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }

    button {
      padding: 10px;
      background: #28a745;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }

    ul {
      list-style-type: none;
      padding: 0;
    }

    li {
      padding: 10px;
      border-bottom: 1px solid #eee;
      display: flex;
      justify-content: space-between;
    }

    .completed {
      text-decoration: line-through;
      color: gray;
    }

    .delete-btn {
      background: #dc3545;
      margin-left: 10px;
    }
  </style>
</head>
<body>

  <div class="container">
    <h2>Task Tracker</h2>
    <input type="text" id="taskInput" placeholder="Enter a task" />
    <button onclick="addTask()">Add</button>
    <ul id="taskList"></ul>
  </div>

  <script>
    const taskInput = document.getElementById("taskInput");
    const taskList = document.getElementById("taskList");

    window.onload = () => {
      const storedTasks = JSON.parse(localStorage.getItem("tasks")) || [];
      storedTasks.forEach(task => addTaskToDOM(task.text, task.completed));
    };

    function addTask() {
      const taskText = taskInput.value.trim();
      if (taskText === "") return;

      addTaskToDOM(taskText, false);
      saveTasks();
      taskInput.value = "";
    }

    function addTaskToDOM(text, completed) {
      const li = document.createElement("li");
      const span = document.createElement("span");
      span.textContent = text;
      if (completed) span.classList.add("completed");
      span.onclick = () => {
        span.classList.toggle("completed");
        saveTasks();
      };

      const delBtn = document.createElement("button");
      delBtn.textContent = "Delete";
      delBtn.className = "delete-btn";
      delBtn.onclick = () => {
        li.remove();
        saveTasks();
      };

      li.appendChild(span);
      li.appendChild(delBtn);
      taskList.appendChild(li);
    }

    function saveTasks() {
      const tasks = [];
      taskList.querySelectorAll("li").forEach(li => {
        const text = li.querySelector("span").textContent;
        const completed = li.querySelector("span").classList.contains("completed");
        tasks.push({ text, completed });
      });
      localStorage.setItem("tasks", JSON.stringify(tasks));
    }
  </script>

</body>
</html>
