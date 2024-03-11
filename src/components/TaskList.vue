<template>
  <div>
    <h2>Todo List</h2>

    <div class="add-task">
      <input type="text" v-model="taskText" placeholder="Add new task" class="add-task-input" />
      <button @click="addTask" class="add-task-button">Add Task</button>
    </div>

    <table class="centered-table">

      <thead>
        <tr>
          <th>Task</th>
          <th>Creation Date</th>
          <th>Completed</th>
          <th>Update</th>
          <th>Remove</th> <!-- New column for remove option -->
        </tr>
      </thead>

      <tbody>
        <tr v-for="task in tasks" :key="task.id">
          <td>{{ task.text }}</td>
          <td>{{ task.createdAt }}</td>
          <td>
            <input type="checkbox" v-model="task.completed" @change="updateTask(task)">
          </td>
          <td>
            <input type="text" v-model="task.text" @change="updateTask(task)">
          </td>
          <td>
            <button @click="removeTask(task.id)">Remove</button> <!-- Remove button -->
          </td>
        </tr>
      </tbody>

    </table>
  </div>

</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      tasks: [],
      taskText: ''
    };
  },
  created() {
    this.fetchTasks();
  },
  methods: {
    async fetchTasks() {
      try {
        const response = await axios.get('http://localhost:3000/tasks');
        this.tasks = response.data;
      } catch (error) {
        console.error('Error fetching tasks:', error);
      }
    },

    async addTask() {
  if (!this.taskText) return;
  try {
    // Get current date
    const currentDate = new Date();
    // Format date as YYYY-MM-DD
    const formattedDate = currentDate.toISOString().split('T')[0];
    // Send task data with formatted date
    await axios.post('http://localhost:3000/tasks', {
      text: this.taskText,
      completed: false,
      createdAt: formattedDate
    });
    this.taskText = '';
    this.fetchTasks(); // Fetch tasks after adding a new task
  } catch (error) {
    console.error('Error adding task:', error);
  }
},

    async updateTask(task) {
      try {
        await axios.patch(`http://localhost:3000/tasks/${task.id}`, {
          text: task.text,
          completed: task.completed
        });
      } catch (error) {
        console.error('Error updating task:', error);
      }
    },

    async removeTask(taskId) {
      try {
        await axios.delete(`http://localhost:3000/tasks/${taskId}`);
        this.tasks = this.tasks.filter(task => task.id !== taskId);
      } catch (error) {
        console.error('Error removing task:', error);
      }
    }

  }
};
</script>


<style>
.centered-table {
  margin: 0 auto; /* Center the table horizontally */
  border-collapse: collapse; /* Collapse borders into a single border */
  width: 50%; /* Set the width of the table */
}

.centered-table th,
.centered-table td {
  border: 1px solid #120f0f; /* Add border to table cells */
  padding: 8px; /* Add padding to table cells */
  text-align: left; /* Align text to the left within table cells */
}

.centered-table th {
  background-color: #2c3e50;
  color:aliceblue; /* Set background color for table header */
}

button {
  background-color: #2c3e50; /* Button background color */
  color: white; /* Button text color */
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background-color: #2cb16e; /* Darker shade for hover effect */
}
</style>
