<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <div class="wrap">
    <aside class="wrapper">
      <nav>
        <RouterLink to="/form">表单</RouterLink>
        <RouterLink to="/array">数组</RouterLink>
      </nav>
    </aside>
    <main>
      <RouterView />
    </main>
  </div>
</template>

<style scoped>
.wrap{
  display: grid;
  min-height: 100vh;
  grid-template-columns: minmax(200px, 25%) 1fr;
  grid-template-rows: 1fr auto;
}
header {
  line-height: 1.5;
  max-height: 100vh;
}


nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

</style>
