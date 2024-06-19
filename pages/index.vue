<template>
  <div>
    <input
      v-model="searchQuery"
      placeholder="Search users..."
      class="p-xs w-full"
    />
    <ul class="user-list">
      <li
        v-for="(user, index) in filteredUsers"
        :key="index"
        :class="{ selected: index === highlightedIndex }"
        class="p-xxs fs-m"
        @click="router.push(`/users/${user.id}`)"
        @mouseenter="highlightedIndex = index"
        @mouseleave="highlightedIndex = -1"
      >
        {{ user.name }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, watch, onMounted, onBeforeUnmount } from "vue";
import { useRouter } from "vue-router";

// Исходный массив строк
const users = [
  { id: 1, name: "User" },
  { id: 2, name: "User1" },
  { id: 3, name: "User2" },
  { id: 4, name: "User3" },
  { id: 5, name: "User4" },
  { id: 6, name: "User5" },
  { id: 7, name: "User6" },
  { id: 8, name: "User7" },
  { id: 9, name: "User8" },
  { id: 10, name: "User9" },
  { id: 11, name: "User10" },
  { id: 12, name: "User11" },
  { id: 13, name: "User12" },
];

const searchQuery = ref("");
const highlightedIndex = ref(0);
const router = useRouter();
let debounceTimeout = null;

watch(searchQuery, (newVal) => {
  if (newVal.length < 3) {
    filteredUsers.value = [];
    return;
  }
  clearTimeout(debounceTimeout);
  debounceTimeout = setTimeout(() => {
    performSearch();
  }, 600);
});

const performSearch = () => {
  filteredUsers.value = [];
  for (const user of users) {
    if (searchQuery.value !== "") {
      if (user.name.toLowerCase().includes(searchQuery.value.toLowerCase())) {
        filteredUsers.value.push(user);
      }
    }
    if (filteredUsers.value.length === 10) break;
  }
};

const filteredUsers = ref([]);

const handleKeyDown = (event) => {
  if (event.key === "ArrowUp") {
    highlightedIndex.value =
      highlightedIndex.value > 0 ? highlightedIndex.value - 1 : 0;
  } else if (event.key === "ArrowDown") {
    highlightedIndex.value =
      highlightedIndex.value < filteredUsers.value.length - 1
        ? highlightedIndex.value + 1
        : filteredUsers.value.length - 1;
  } else if (event.key === "Enter") {
    if (
      filteredUsers.value.length > 0 &&
      highlightedIndex.value < filteredUsers.value.length
    ) {
      const selectedUser = filteredUsers.value[highlightedIndex.value];
      router.push(`/users/${selectedUser.id}`);
    }
  }
};

onMounted(() => {
  window.addEventListener("keydown", handleKeyDown);
});

onBeforeUnmount(() => {
  window.removeEventListener("keydown", handleKeyDown);
});
</script>

<style scoped>
.user-list li {
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.user-list li.selected {
  background-color: #d3d3d3;
}

.user-list li:hover {
  background-color: #78dfbd94;
}
</style>
