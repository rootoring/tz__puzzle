<template>
  <div>
    <h1>User Details</h1>
    <p>User ID: {{ userId }}</p>
  </div>
</template>

<script setup>
import { useRoute } from "vue-router";

const route = useRoute();
const userId = route.params.id;
</script>
