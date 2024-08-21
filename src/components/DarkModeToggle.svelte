<script>
  import { onMount } from 'svelte';

  let isDarkMode = false;

  onMount(() => {
    isDarkMode = localStorage.getItem('dark-mode') === 'true';
    applyDarkMode(isDarkMode);
  });

  function toggleDarkMode() {
    isDarkMode = !isDarkMode;
    applyDarkMode(isDarkMode);
    localStorage.setItem('dark-mode', isDarkMode.toString());
  }

  function applyDarkMode(darkMode) {
    document.documentElement.classList.toggle('dark', darkMode);
  }
</script>

<button on:click={toggleDarkMode}>
  {#if isDarkMode}
    ☀️ Light Mode
  {:else}
    🌙 Dark Mode
  {/if}
</button>

<style>
  button {
    padding: 0.5em 1em;
    border: none;
    border-radius: 5px;
    background-color: var(--button-background);
    color: var(--button-color);
    cursor: pointer;
    transition: background-color 0.3s;
  }

  button:hover {
    background-color: var(--button-background-hover);
  }
</style>
