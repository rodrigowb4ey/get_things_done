<template>
  <nav class="py-8 text-center">
    <RouterLink 
      v-for="link in ['/', '/tasks', '/workouts']" 
      :key="link"
      :to="link"
      class="mx-4 text-slate-700 no-underline hover:text-slate-900 transition-colors"
      :class="{ 'font-bold': $route.path === link }"
    >
      {{ link === '/' ? 'Home' : link.slice(1).charAt(0).toUpperCase() + link.slice(2) }}
    </RouterLink>
  </nav>

  <RouterView />
</template>