<!-- App.svelte -->
<script>
  import { onMount, afterUpdate } from 'svelte';
  import LyricsDisplay from './LyricsDisplay.svelte';
  import Sidebar from './Sidebar.svelte';
  import { selectedData } from './selectedDataStore.js';
  import ParentContainer from './ParentContainer.svelte';
  import LyricsInput from './LyricsInput.svelte';

  let loadedData = null;
  let savedDataTitles = [];

  onMount(() => {
    // Load the saved data titles when the app starts
    loadSavedDataTitles();
  });

  afterUpdate(() => {
    // Observe changes in selectedData and react accordingly
    $selectedData && loadSavedData($selectedData);
  });

  function loadSavedDataTitles() {
    // Load saved data titles from localStorage
    const titles = Object.keys(localStorage);
    savedDataTitles = titles;
  }

  function loadSavedData(title) {
    // Load the data based on the selected title
    const key = title;
    const savedDataString = localStorage.getItem(key);

    if (savedDataString !== null) {
      loadedData = JSON.parse(savedDataString);
    } else {
      // Handle the case when data with the specified title is not found
      loadedData = null;
    }
  }
</script>

<style>
  .lyrics-input-container {
    position: fixed; /* Fixed positioning relative to the viewport */
    right: 0; /* Align to the right edge */
    bottom: 0; /* Align to the bottom edge */
    margin: 20px; /* Add some margin for spacing from the edges */
    background-color: rgba(255, 255, 255, 0.9); /* Optional: Add a background color with some transparency */
    padding: 10px; /* Add some padding around the content */
    border-radius: 8px; /* Optional: Add rounded corners */
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Optional: Add a subtle shadow for better visibility */
  }
</style>

<main>
  <Sidebar savedDataList={savedDataTitles} selectedData={$selectedData} />
  <div class="content">
    {#if loadedData !== null}
      <!-- Display loaded data here -->
      <h2>{loadedData.title}</h2>
      <p>{loadedData.lyrics}</p>
      <!-- Add more elements as needed to display other data properties -->
    {:else}
      <p>No data loaded.</p>
    {/if}
  </div>
  <div class="lyrics-input-container">
    <LyricsInput />
  </div>
</main>  

