const taskInput = document.getElementById("taskInput");
const taskList = document.getElementById("taskList");
const filterButtons = document.querySelectorAll(".filter-buttons button");
const warningMessage = document.getElementById("warning");

function addTask() {
  const taskText = taskInput.value.trim();
  if (taskText === "") {
    warningMessage.classList.remove("hidden");
    return;
  }

  warningMessage.classList.add("hidden");

  const li = document.createElement("li");
  li.innerHTML = `
    <input type="checkbox" onchange="markCompleted(event)">
    <span>${taskText}</span>
    <button class="delete-btn" onclick="deleteTask(event)">Delete</button>
  `;
  taskList.appendChild(li);
  taskInput.value = "";
}

function deleteTask(event) {
  event.stopPropagation();
  const btn = event.target;
  const li = btn.parentElement;
  li.remove();
}

function markCompleted(event) {
  const checkbox = event.target;
  const li = checkbox.parentElement;
  li.classList.toggle("completed");
}

function filterTasks(filter) {
  currentFilter = filter;
  filterButtons.forEach((button) => button.classList.remove("active"));
  event.target.classList.add("active");

  const tasks = taskList.querySelectorAll("li");

  tasks.forEach((task) => {
    if (filter === "all") {
      task.style.display = "flex";
    } else if (filter === "completed") {
      task.style.display = task.classList.contains("completed") ? "flex" : "none";
    } else if (filter === "uncompleted") {
      task.style.display = task.classList.contains("completed") ? "none" : "flex";
    }
  });
}

function deleteAllTasks() {
  taskList.innerHTML = ""; // Remove all tasks from the list
}

filterButtons.forEach((button) => {
  button.addEventListener("click", () => filterTasks(button.dataset.filter));
});