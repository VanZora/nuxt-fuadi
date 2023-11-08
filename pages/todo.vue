<script setup>

const todos = ref([
  {
    id: 1,
    title: 'Testing'
  },
  {
    id: 2,
    title: 'Lorem Ipsum'
  }
])

const newTodo = ref("")
function handleNewTodo() {
  todos.value.push({
    id: todos.value.length + 1,
    title: newTodo.value
  })
  newTodo.value = ""
}

function handleDeleteTodo(item) {
  todos.value = todos.value.filter((todo) => todo.id != item.id)
}

const selectedTodo = ref(null)

function handleSelectTodo(item) {
  selectedTodo.value = item
  newTodo.value = item.title
}
function handleUpdateTodo(item) {
  handleDeleteTodo(selectedTodo.value)

  todos.value.push({
    id: selectedTodo.value.id,
    title: newTodo.value,
  })

  newTodo.value = ""
  selectedTodo.value = null
}
</script>

<!-- :value="newTodo" 
  @input="(event) => (newTodo = event.target.value)" -->

<template>
  <v-app>
    <v-main>
      <v-container>
        <div>
                    <v-btn variant="text" prepend-icon="mdi-arrow-left" @click="navigateTo('/')">Menu Utama</v-btn>
                </div>
        <!-- <input placeholder="New Todo ..." v-model="newTodo"/> -->
        <v-text-field placeholder="New Todo ..." v-model="newTodo" label="New Todo ..."></v-text-field>
        <v-btn v-if="!selectedTodo" class="mr-4" prepend-icon="mdi-content-save-outline" color="primary"
          @click="handleNewTodo()">
          Save
        </v-btn>
        <v-btn v-else prepend-icon="mdi-pencil-outline" color="primary" @click="handleUpdateTodo(item)">
          Update
        </v-btn>

        <v-list lines="one">
          <v-list-item v-for="item in todos" :key="item.id">
            <div class="d-flex align-center">
              <div>{{ item.title }}</div>
              <v-spacer />

              <v-btn @click="handleSelectTodo(item)" size="small" variant="flat" icon="mdi-pencil-outline">

              </v-btn>

              <v-btn @click="handleDeleteTodo(item)" size="small" variant="flat" icon="mdi-delete-outline">
              </v-btn>

            </div>
          </v-list-item>
        </v-list>
        <!-- <button >Submit</button> -->
      </v-container>
    </v-main>
  </v-app>
</template>
