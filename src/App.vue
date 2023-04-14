<template>
  <div class="todo-app">
    <h1>Todo App</h1>

    <div class="input-group">
      <input type="text" placeholder="Enter new folder" v-model="newFolder" @keyup.enter="addFolder" />
      <button @click="addFolder">Add Folder</button>
    </div>

    <div class="folders">
      <ul>
        <li v-for="folder in folders" :key="folder.id">
          <h2 class="folder-label">{{ folder.title }} <button class="folder-button" @click="deleteFolder(folder)">X</button></h2>
          <div class="input-group">
            <input type="text" placeholder="Enter new task" v-model="newTodo" @keyup.enter="addTodo(folder.id)" />
            <button class="add-button" @click="addTodo(folder.id)">Add</button>
          </div>
          <div v-for="todo in todosByFolder(folder.id)" :key="todo.id">
            <input type="checkbox" :id="todo.id" v-model="todo.completed" />
            <label :for="todo.id" :class="{ completed: todo.completed }">{{ todo.title }}</label>
            <span class="created-date">({{ formatDate(todo.createdAt) }})</span>
            <button class="delete-button" @click="deleteTodo(todo)">Delete</button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      folders: [],
      newFolder: '',
      todos: [],
      newTodo: '',
    };
  },

  computed: {
    todosByFolder: function() {
      return function(folderId) {
        return this.todos.filter((todo) => todo.folderId === folderId);
      };
    },
  },

  methods: {
    addFolder: function() {
      const title = this.newFolder.trim();
      if (title) {
        const folder = {
          id: Date.now(),
          title,
        };
        this.folders.push(folder);
        this.newFolder = '';
      }
    },

    deleteFolder: function(folder) {
      const index = this.folders.indexOf(folder);
      if (index !== -1) {
        this.folders.splice(index, 1);
        this.todos = this.todos.filter((todo) => todo.folderId !== folder.id);
      }
    },

    addTodo: function(folderId) {
      const title = this.newTodo.trim();
      if (title) {
        const todo = {
          id: Date.now(),
          folderId,
          title,
          completed: false,
          createdAt: new Date(),
        };
        this.todos.push(todo);
        this.newTodo = '';
      }
    },

    deleteTodo: function(todo) {
      const index = this.todos.indexOf(todo);
      if (index !== -1) {
        this.todos.splice(index, 1);
      }
    },

    formatDate: function(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' };
      return new Date(date).toLocaleDateString(undefined, options);
    },
  },
};
</script>

<style scoped>
body {
  font-family: Arial, sans-serif;
  background-color: #f0f0f0;
}

.todo-app {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

h1 {
  text-align: center;
  color: #333;
}

.input-group {
  display: flex;
justify-content: center;
align-items: center;
margin: 1.25rem 0;
}

.input-group input[type="text"] {
font-size: 1rem;
padding: 10px;
border: 1px solid #ccc;
border-radius: 5px;
margin-right: 10px;
width: 300px;
}

.input-group button {
font-size: 1rem;
padding: 10px;
border: none;
border-radius: 5px;
background-color: #b2cc2e;
color: #fff;
cursor: pointer;
transition: background-color 0.3s ease;
}

.input-group button:hover {
background-color: #aea327;
}

.folders {
margin-bottom: 1rem;
}

.folders ul {
list-style: none;
padding: 0;
margin: 0;
}

.folders li {
margin: 10px 0;
padding: 10px;
background-color: #407bf838;
border: 1px solid #ccc;
border-radius: 5px;
}

.folders li h2 {
font-size: 1.5rem;
color: #333;
display: inline;
margin-right: 1rem;
}

.folders li button {
font-size: 1rem;
padding: 5px;
border: none;
border-radius: 5px;
color: #fff;
cursor: pointer;
transition: background-color 0.3s ease;
background-color: #e74c3c;
}

.folders li button:hover {
background-color: #c0392b;
}

.todos input[type="checkbox"] {
margin-right: 5px;
}

.folder-label {
  display: block !important;
  text-align: center;
}

.folder-button {
  float: right;
  padding-inline: 8px !important;
}

.add-button {
  background-color: #2bc041 !important;
}

.delete-button {
  margin: 1rem !important;
}

.folders li label.completed {
text-decoration : line-through;
color: #999;
}

.created-date {
margin-left: 1rem;
font-size: 0.75rem;
}
</style>