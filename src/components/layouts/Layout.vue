<script setup></script>

<template>
  <header>
    <h1 class="text-gradient">SMOLGRAM</h1>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <small>Created by</small>
    <a href="https://www.github.com/AnushaNC86" target="_blank">
      <p>Anusha</p>
      <i class="fa-brands fa-github"></i>
    </a>
  </footer>
</template>

<style scoped>
header,
footer,
main {
  padding: 1rem;
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
}

main {
  flex: 1;
}

footer {
  display: flex;
  border-top: 1px solid var(--border-highlight);
  gap: 0.5rem;
  align-items: center;
  justify-content: center;
  padding: 1rem 0;
  padding-bottom: 3rem;
}

footer a {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.3rem;
  padding-right: 0.5rem;
  background: var(--background-muted);
  border-radius: 4rem;
  border: 1px solid transparent;
  transition-duration: 200ms;
  text-decoration: none;
}

footer a:hover {
  border-color: var(--color-link);
}

footer a img {
  max-width: 30px;
  aspect-ratio: 1 / 1;
  border-radius: 100%;
}
</style>
