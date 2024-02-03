<!-- Sidebar.svelte -->

<script>
  import { selectedData } from './selectedDataStore.js';

  export let savedDataList; // An array of saved data titles

  function selectData(title) {
    $selectedData = title; // Update the selectedData store
  }
  let isModalOpen = false; // Whether the modal is open


  function deleteData() {
  if (selectedData !== null) {
    const key = selectedData; // Assuming selectedData is the title of the data

    // Display a confirmation prompt
    const confirmDelete = confirm('Are you sure you want to delete this data?');

    if (confirmDelete) {
      // User confirmed, delete the data
      localStorage.removeItem(key);

      // Clear the selectedData store
      $selectedData = null;

      // Update the list of saved data titles
      loadSavedDataTitles();
    }
  }
}
</script>

<div class="sidebar">
  <h2>Select Saved Data</h2>
  <ul>
    {#each savedDataList as title}
      <li
        class:active={selectedData === title}
        on:click={() => selectData(title)}
      >
        {title}
      </li>
    {/each}
  </ul>
</div>


<style>
  .sidebar {
    width: 250px;
    background-color: #f0f0f0;
    padding: 10px;
  }

  ul {
    list-style: none;
    padding: 0;
  }

  li {
    cursor: pointer;
    padding: 5px;
  }

  li.active {
    background-color: #ccc;
  }
</style>