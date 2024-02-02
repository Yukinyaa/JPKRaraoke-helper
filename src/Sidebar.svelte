<!-- Sidebar.svelte -->

<script>
  import { selectedData } from './selectedDataStore.js';

  export let savedDataList; // An array of saved data titles

  function selectData(title) {
    $selectedData = title; // Update the selectedData store
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