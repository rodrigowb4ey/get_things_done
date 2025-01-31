<template>
  <nav>
    <RouterLink to="/">Home</RouterLink> | <RouterLink to="/tasks">Tasks</RouterLink> |
    <RouterLink to="/workouts">Workouts</RouterLink>
  </nav>

  <RouterView />
</template>

<style scoped>
nav {
  padding: 2rem;
  text-align: center;
}

nav a {
  margin: 0 1rem;
  text-decoration: none;
  color: #2c3e50;
}

nav a.router-link-active {
  font-weight: bold;
}
</style>
