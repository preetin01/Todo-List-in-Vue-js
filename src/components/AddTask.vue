<script>
import axios from 'axios';

export default {
  data() {
    return {
      taskText: ''
    };
  },
  methods: {
    async addTask() {
      if (!this.taskText) return;
      try {
        await axios.post('http://localhost:3000/tasks', {
          text: this.taskText,
          completed: false,
          createdAt: new Date().toISOString()
        });
        this.taskText = '';
        this.$emit('task-added');
      } catch (error) {
        console.error('Error adding task:', error);
      }
    }
  }
};
</script>

<style>
*{
  background-color: #6fa388;
}
.add-task {
  margin:auto;
  display: flex;
  width:30%;
  align-items: center;
  margin-bottom: 20px; 
}

.add-task-input {
  flex: 1;
  margin-right: 10px; 
  padding: 8px;
  border: 1px solid #120f0f; 
  border-radius: 4px;
}

.add-task-button {
  padding: 8px 16px;
  background-color: #2c3e50; 
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.add-task-button:hover {
  background-color: #2cb16e; 
}
</style>
