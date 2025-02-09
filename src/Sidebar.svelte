<!-- Sidebar.svelte -->

<script>

  import { selectedData } from './selectedDataStore.js';
  import JSZip from 'jszip';
  export let savedDataList; // An array of saved data titles

  function selectData(title) {
    $selectedData = title; // Update the selectedData store
  }

  function deleteData() {
    if (selectedData !== null) {
      const key = selectedData;

      if (confirm('Are you sure you want to delete this data?')) {
        localStorage.removeItem(key);
        $selectedData = null;
        // loadSavedDataTitles(); // Refresh the list
      }
    }
  }

  async function exportDatabase() {
    const zip = new JSZip();

    // Loop through localStorage keys and save them as txt files
    for (let i = 0; i < localStorage.length; i++) {
      const key = localStorage.key(i);
      const content = localStorage.getItem(key);
      zip.file(`${key}.txt`, content);
    }

    const blob = await zip.generateAsync({ type: 'blob' });
    const link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download = 'lyrics_backup.zip';
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
  }

  function importDatabase(event) {
    const file = event.target.files[0];
    if (!file) return;

    const zip = new JSZip();
    zip.loadAsync(file).then((zip) => {
      Object.keys(zip.files).forEach((filename) => {
        zip.files[filename].async('text').then((content) => {
          const title = filename.replace('.txt', '');
          localStorage.setItem(title, content);
          // loadSavedDataTitles(); // Refresh list
        });
      });
    });
  }
</script>
<div class="sidebar">
  <h2>Select Saved Data</h2>
  <ul>
    {#each savedDataList as title}
      <li class:active={selectedData === title} on:click={() => selectData(title)}>
        {title}
      </li>
    {/each}
  </ul>

  <div class="button-container">
    <button class="action-button" on:click={exportDatabase}>Export Zip</button>
    <button class="action-button" on:click={() => document.getElementById('importFile').click()}>
      Import Zip
    </button>
    <input id="importFile" type="file" accept=".zip" on:change={importDatabase} style="display: none;" />
  </div>
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

  .button-container {
    margin-top: 10px;
    display: flex;
    gap: 10px;
    justify-content: space-between;
  }

  .action-button {
    flex: 1;
    padding: 10px;
    cursor: pointer;
    background-color: #ffffff;
    color: #333333;
    border: black;
    border-radius: 5px;
    text-align: center;
    width: 120px;
    font-size: 14px;
    font-weight: bold;
    user-select: none;
  }

  .action-button:hover {
    background-color: #bbbbbb;
  }
</style>