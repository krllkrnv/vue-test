<template>
  <div class="app">
    <h1>Users</h1>

    <div v-if="isLoading">Loading...</div>
    <div v-else-if="error">{{ error }}</div>

    <div v-else>
      <UserSearch v-model="query" />
      <div class="users-list">
        <UserCard
          v-for="user in filteredUsers"
          :key="user.id"
          :user="user"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref, onMounted } from 'vue'
import { fetchUsers } from './api/mock.js'
import UserSearch from './components/UserSearch.vue'
import UserCard from './components/UserCard.vue'

const users = ref([])
const isLoading = ref(false)
const error = ref('')
const query = ref('')

const filteredUsers = computed(() => {
  const normalizedQuery = query.value.trim().toLowerCase()

  if (!normalizedQuery) {
    return users.value
  }

  return users.value.filter((user) => {
    return user.name.toLowerCase().includes(normalizedQuery)
  })
})

onMounted(() => {
  loadUsers()
})

async function loadUsers() {
  isLoading.value = true
  error.value = ''

  try {
    users.value = await fetchUsers()
  } catch (err) {
    error.value = err.message
  } finally {
    isLoading.value = false
  }
}
</script>

<style scoped>
.app {
  padding: 20px;
  max-width: 900px;
  margin: 0 auto;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}

h1 {
  color: #333;
  margin-bottom: 20px;
}

.users-list {
  display: grid;
  gap: 12px;
}
</style>
