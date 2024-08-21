<script>
  import { onMount } from 'svelte';

  let years = [2024, 2023, 2022];
  let selectedYear = years[0];
  let weeks = [
    { value: "", label: "--Select Week--" },
    { value: "1", label: "Week 1" },
    { value: "2", label: "Week 2" },
    { value: "3", label: "Week 3" },
    { value: "4", label: "Week 4" },
    { value: "5", label: "Week 5" },
    { value: "6", label: "Week 6" },
    { value: "7", label: "Week 7" },
    { value: "8", label: "Week 8" }
  ];
  let selectedWeek = "";
  let events = [];
  let filteredEvents = [];
  let selectedEvent = "";
  let teams = [];
  let eventCode = "";

  const API_KEY = "FkG1dAr0pfcoJ40y9s0ww8DqV06swWCbZNFFgfbVTlLK4pKwoeTYdXfZ65rFr8Ms ";

  async function fetchEvents() {
    if (selectedYear) {
      try {
        const res = await fetch(`https://www.thebluealliance.com/api/v3/events/${selectedYear}`, {
          headers: { "X-TBA-Auth-Key": API_KEY }
        });
        if (res.ok) {
          const allEvents = await res.json();
          if (selectedWeek === "7") {
            filteredEvents = allEvents.filter(event => event.week === 6);
          } else if (selectedWeek === "8") {
            filteredEvents = allEvents.filter(event =>
              event.name.toLowerCase().includes("championship") ||
              event.name.toLowerCase().includes("preseason") ||
              event.name.toLowerCase().includes("offseason")
            );
          } else {
            filteredEvents = allEvents.filter(event => event.week === parseInt(selectedWeek));
          }
        } else {
          console.error("Failed to fetch events:", res.statusText);
        }
      } catch (error) {
        console.error("Error fetching events:", error);
      }
    }
  }

  async function fetchTeams() {
    const eventToFetch = selectedEvent || eventCode;
    if (eventToFetch) {
      try {
        const res = await fetch(`https://www.thebluealliance.com/api/v3/event/${eventToFetch}/teams`, {
          headers: { "X-TBA-Auth-Key": API_KEY }
        });
        if (res.ok) {
          teams = await res.json();
        } else {
          console.error("Failed to fetch teams:", res.statusText);
        }
      } catch (error) {
        console.error("Error fetching teams:", error);
      }
    }
  }

  onMount(() => {
    if (selectedYear) {
      fetchEvents(); // Fetch events initially based on default year
    }
  });

  $: selectedYear, selectedWeek, fetchEvents();
</script>

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
    background-color: var(--background-color);
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5);
  }
  .dropdowns {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin-bottom: 20px;
    width: 100%;
    max-width: 800px;
  }
  .dropdown {
    margin: 10px;
    width: 180px;
    background-color: var(--dropdown-background-color);
    border: 1px solid var(--dropdown-border-color);
    border-radius: 5px;
  }
  .dropdown label {
    display: block;
    padding: 5px;
    color: var(--text-color);
    background-color: var(--box-border-color);
    border-radius: 5px 5px 0 0;
    text-align: center;
  }
  select {
    width: 100%;
    padding: 10px;
    border: none;
    border-radius: 0 0 5px 5px;
    background-color: var(--box-background-color);
    color: var(--text-color);
  }
  .input-section {
    margin-bottom: 20px;
    width: 100%;
    max-width: 800px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .input-section label {
    display: block;
    color: var(--text-color);
    margin-bottom: 5px;
  }
  .input-section input, .input-section button {
    width: 100%;
    padding: 10px;
    border: 1px solid var(--button-background);
    border-radius: 5px;
  }
  .input-section input {
    margin-bottom: 10px;
    background-color: var(--button-background);
    color: var(--button-color);
  }
  .input-section button {
    background-color: var(--button-background);
    color: var(--button-color);
    cursor: pointer;
  }
  .input-section button:hover {
    background-color: var(--button-background-hover);
  }
  .box {
    display: inline-block;
    border: 1px solid var(--box-border-color);
    padding: 10px;
    margin: 5px;
    text-align: center;
    background-color: var(--box-background-color);
    color: var(--text-color);
    width: 150px;
    height: 100px;
    border-radius: 5px;
    box-sizing: border-box;
  }
  .box h3 {
    margin: 0;
    font-size: 18px;
  }
  .box p {
    margin: 0;
    font-size: 14px;
  }
</style>

<div class="container">
  <!-- Dropdowns Section -->
  <div class="dropdowns">
    <!-- Year Dropdown -->
    <div class="dropdown">
      <label for="year">Year:</label>
      <select id="year" bind:value={selectedYear} on:change={() => {
        selectedWeek = ""; // Reset week and event when year changes
        selectedEvent = "";
        fetchEvents();
      }}>
        {#each years as year}
          <option value={year}>{year}</option>
        {/each}
      </select>
    </div>

    <!-- Week Dropdown -->
    {#if selectedYear}
      <div class="dropdown">
        <label for="week">Week:</label>
        <select id="week" bind:value={selectedWeek} on:change={() => {
          selectedEvent = ""; // Reset event when week changes
          fetchEvents();
        }}>
          {#each weeks as week}
            <option value={week.value}>{week.label}</option>
          {/each}
        </select>
      </div>
    {/if}

    <!-- Event Dropdown -->
    {#if selectedWeek}
      <div class="dropdown">
        <label for="event">Event:</label>
        <select id="event" bind:value={selectedEvent} on:change={fetchTeams}>
          <option value="">--Select Event--</option>
          {#each filteredEvents as event}
            <option value={event.key}>{event.name}</option>
          {/each}
        </select>
      </div>
    {/if}
  </div>

  <!-- Enter Event Code Input -->
  <div class="input-section">
    <label for="eventCode">Enter Event Code:</label>
    <input id="eventCode" type="text" bind:value={eventCode} placeholder="e.g., 2024mibro" />
    <button on:click={fetchTeams}>Get Teams</button>
  </div>

  <!-- Display Teams -->
  <div>
    {#each teams as team}
      <div class="box">
        <h3>{team.team_number}</h3>
        <p>{team.nickname}</p>
      </div>
    {/each}
  </div>
</div>
