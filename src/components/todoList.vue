<template>
  <v-container class="todo-container py-4" max-width="600px">
    <v-card>
      <v-card-title class="headline">
        <div class="title-container">
          <v-img :src="appIcon" class="app-icon"></v-img>
          <span class="title-text">To-do Lists</span>
        </div>
      </v-card-title>
      <v-card-subtitle>
        <v-text-field
          v-model="newItem"
          placeholder="Add a new task..."
          outlined
          dense
          hide-details
          class="w-100 darker-text-field"
          prepend-icon="mdi-plus"
        ></v-text-field>
      </v-card-subtitle>
      <v-card-actions class="center-actions">
        <!-- Using a regular HTML button -->
        <button @click="addItem" class="add-button">Add Task</button>
      </v-card-actions>
      <v-list>
        <!-- Rendering each item in the list -->
        <v-list-item v-for="(item, index) in items" :key="index" class="v-list-item--clickable">
          <v-list-item-content class="d-flex align-center justify-space-between">
            <div class="d-flex align-center task-item">
              <v-checkbox
                v-model="item.checked"
                @click.stop="toggleCheck(index)"
                class="mr-2"
              ></v-checkbox>
              <span :class="{'line-through': item.checked}" class="task-text">{{ item.text }}</span>
            </div>
            <div class="d-flex align-center">
              <v-icon
                @click.stop="editItem(index)"
                class="mr-2"
                color="blue"
              >
                mdi-pencil
              </v-icon>
              <v-icon
                @click.stop="deleteItem(index)"
                color="red"
              >
                mdi-delete
              </v-icon>
            </div>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      newItem: '', // Holds the new task entered by the user
      items: [],   // Holds the list of tasks
      appIcon: require('@/assets/icons8-book-64.png') // Replace with your actual icon path
    };
  },
  methods: {
    addItem() {
      // Check if the newItem is empty
      if (this.newItem.trim() === '') {
        alert('The textbox is empty. Please enter a task.');
        return;
      }
      // Add the new task to the items array
      this.items.push({ text: this.newItem, checked: false });
      this.newItem = ''; // Clear the text field
      this.save(); // Save the list to local storage
    },
    toggleCheck(index) {
      // Toggle the checked status of the task
      this.items[index].checked = !this.items[index].checked;
      this.save();
    },
    editItem(index) {
      const newText = prompt('Edit the task:', this.items[index].text);
      if (newText !== null && newText.trim() !== '') {
        this.items[index].text = newText;
        this.save();
      }
    },
    deleteItem(index) {
      // Remove the task from the list
      this.items.splice(index, 1);
      this.save();
    },
    save() {
      // Save the items list to local storage
      localStorage.setItem('items', JSON.stringify(this.items));
    },
    load() {
      // Load the items list from local storage
      const savedItems = localStorage.getItem('items');
      if (savedItems) {
        this.items = JSON.parse(savedItems);
      }
    }
  },
  mounted() {
    this.load(); // Load the list when the component is mounted
  }
};
</script>

<style scoped>
.todo-container {
  margin-top: 50px;
  background-color: rgb(30, 41, 59);
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.app-icon {
  width: 40px;
  height: 40px;
}

.v-card-title {
  display: flex;
  justify-content: center;
  font-weight: bold;
}

.title-container {
  display: flex;
  align-items: center;
}

.title-text {
  margin-left: 10px;
}

.darker-text-field input {
  color: rgb(203 213 225); /* Optional: change text color to white for better contrast */
}

.v-btn {
  font-weight: 600;
}

.v-list-item--clickable {
  cursor: pointer;
}

.v-list-item-content {
  flex: 1;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.v-list-item-action {
  display: flex;
  align-items: center;
}

.v-icon {
  font-size: 20px;
  cursor: pointer;
}

.line-through {
  text-decoration: line-through;
  color: #9e9e9e;
}

.task-text {
  margin-left: 8px;
  vertical-align: middle;
}

.task-item {
  display: flex;
  align-items: center;
}

.checkbox-align {
  margin-bottom: 0; /* Ensures the checkbox aligns properly */
}

/* Styling for the Add button */
.add-button {
  background-color: #2196f3;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  width: 100%;
}

.add-button:hover {
  background-color: #1976d2;
}

/* Center the button within the v-card-actions */
.center-actions {
  display: flex;
  justify-content: center; /* Centers the button horizontally */
}
</style>
