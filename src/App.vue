<template>
  <v-app class="body">
    <v-container>
      <v-row>
        <!-- Center the card on medium and larger screens -->
        <v-col cols="12" md="8" lg="6" class="mx-auto">
          <v-card class="elevation-12 pa-4 rounded-lg white-bg">
            <!-- Toolbar for the header -->
            <v-toolbar color="white" dark flat>
              <v-toolbar-title class="title">My To-Do List</v-toolbar-title>
            </v-toolbar>

            <v-card-text>
              <!-- Form for adding tasks -->
              <v-form @submit.prevent="addTask">
                <v-text-field
                  label="What needs to be done?"
                  v-model="newTask.task"
                  outlined
                  dense
                  clearable
                  required
                  class="white-text-field short-border"
                ></v-text-field>
                <v-btn
                  type="submit"
                  color="primary"
                  class="mt-3 custom-btn align-left"
                >
                  Add Task
                </v-btn>
                <v-alert v-if="alertMessage" type="error" class="mt-3">
                  {{ alertMessage }}
                </v-alert>
              </v-form>

              <!-- List of tasks -->
              <v-list dense two-line class="mt-4">
                <v-list-item-group>
                  <v-list-item
                    v-for="(task, index) in tasks"
                    :key="index"
                    class="my-2 task-border"
                    :class="{'bg-lighten-5': task.completed}"
                  >
                    <v-list-item-content class="d-flex align-center">
                      <v-checkbox
                        v-model="task.completed"
                        class="mr-3"
                        color="primary"
                      ></v-checkbox>
                      <v-list-item-title
                        :style="{'text-decoration': task.completed ? 'line-through' : 'none'}"
                        class="flex-grow-1"
                      >
                        {{ task.task }}
                      </v-list-item-title>
                      <v-list-item-subtitle class="mr-3">{{ task.date }}</v-list-item-subtitle>
                      <v-list-item-action>
                        <div class="d-flex justify-start"> <!-- Align buttons to the left -->
                          <v-btn
                            icon
                            @click.stop="editTask(index)"
                            class="mx-1 blue-btn"
                          >
                            <v-icon>mdi-pencil</v-icon>
                          </v-btn>
                          <v-btn
                            icon
                            @click.stop="removeTask(index)"
                            class="mx-1 red-btn"
                          >
                            <v-icon>mdi-delete</v-icon>
                          </v-btn>
                        </div>
                      </v-list-item-action>
                    </v-list-item-content>
                  </v-list-item>
                </v-list-item-group>
              </v-list>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      newTask: {
        task: "",
        date: "",
        completed: false,
      },
      tasks: [],
      alertMessage: "", // To store the alert message
    };
  },
  methods: {
    getCurrentDateTime() {
      const now = new Date();
      const date = now.toLocaleDateString(); // Format: MM/DD/YYYY
      const time = now.toLocaleTimeString(); // Format: HH:MM:SS AM/PM
      return `${date}, ${time}`;
    },
    addTask() {
      if (this.newTask.task.trim() === "") {
        this.alertMessage = "Please enter a task before adding.";
        return;
      }
      this.alertMessage = ""; // Clear alert message
      this.newTask.date = this.getCurrentDateTime(); // Automatically set current date and time
      this.tasks.push({ ...this.newTask });
      this.newTask.task = "";
      this.newTask.date = "";
      this.newTask.completed = false;
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      const task = this.tasks[index];
      this.newTask = { ...task };
      this.tasks.splice(index, 1); // Remove the task while editing
    },
  },
};
</script>

<style scoped>
/* Background color for the card */
.white-bg {
  background-color: #FFFBDA !important;
}

/* Rounded corners for the card and list items */
.rounded-lg {
  border-radius: 12px !important;
}

/* Optional: Background color for completed tasks */
.bg-lighten-5 {
  background-color: #FFFBDA !important;
}

/* Center the toolbar title and increase font size */
.title {
  text-align: center;
  font-weight: bold;
  font-size: 32px; /* Larger font size */
  letter-spacing: 1px; /* Wider letter spacing */
  color: black; /* Ensure text is visible against white background */
}

/* White text color for text field */
.white-text-field input,
.white-text-field .v-label {
  color: black !important; /* Text color for input */
}

.white-text-field .v-input__control .v-input__slot {
  border-color: black !important; /* Border color for input */
}

/* Shorten the width of the border for the text field */
.short-border .v-input__control .v-input__slot {
  border-width: 1px !important; /* Adjust as needed */
}

/* Custom button styling */
.custom-btn {
  background-color: #4caf50 !important;
  color: white !important;
  width: auto; /* Ensure the button does not stretch to fill the width */
}

.custom-btn:hover {
  background-color: #45a049 !important; /* Darker green on hover */
}

/* Align the button to the left */
.align-left {
  text-align: left !important;
}

/* Blue button styling for edit */
.blue-btn {
  background-color: #2196f3 !important;
  color: white !important;
}

.blue-btn:hover {
  background-color: #1e88e5 !important; /* Darker blue on hover */
}

/* Red button styling for delete */
.red-btn {
  background-color: #f44336 !important;
  color: white !important;
}

.red-btn:hover {
  background-color: #e53935 !important; /* Darker red on hover */
}

/* Border styling for task list items */
.task-border {
  border: 1px solid black; /* Black border */
  background-color: white !important; /* White fill */
  padding: 8px; /* Add some padding */
  margin-bottom: 8px; /* Space between tasks */
  border-radius: 12px; /* Ensure rounded corners */
}

/* Background color for the entire app */
.body {
  background-color: #FFEC9E; /* Light yellow color */
}
</style>
