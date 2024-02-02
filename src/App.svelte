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

