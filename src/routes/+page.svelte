<script>
  import EventPicker from '../components/EventPicker.svelte';
  import DarkModeToggle from '../components/DarkModeToggle.svelte';
</script>

<main>
  <DarkModeToggle />
  <EventPicker />
</main>

<style>
  main {
    padding: 1em;
    max-width: 800px;
    margin: 0 auto;
  }

  /* Add any additional styling needed for the components here */
</style>
