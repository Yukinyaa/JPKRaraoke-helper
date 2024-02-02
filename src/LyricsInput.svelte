<!-- LyricsInput.svelte -->
<script>
  import { slide } from 'svelte/transition';
  let title = '';
  let lyrics = '';
  let isVisible = true;
  let selectedSet = []; // To store the selected set of lines for user identification
  let groupIndex = 2; // Index for the 7th set (0-based index)
  let allLyrics = []; // Assuming this contains all the lyrics lines
  let typesOrder = []; // This will be set based on user selection

  function toggleVisibility() {
    isVisible = !isVisible;
  }

  function parseLyrics() {
    const lines = lyrics.split('\n').filter(line => line.trim() !== '');
    const groupedLines = [];

    for (let i = 0; i < lines.length; i += 3) {
      // Store each line as an object with 'text' and a default 'type'
      const group = lines.slice(i, i + 3).map(line => ({ text: line, type: '' }));
      groupedLines.push(group);
    }

    if (groupedLines.length > groupIndex) {
      selectedSet = groupedLines[groupIndex];
    } else {
      console.error('Not enough groups to display the specified set.');
      selectedSet = [];
    }
  }

  function confirmSelection() {
  // Extract the types from the selectedSet
  const types = selectedSet.map(line => line.type);

  // Check for duplicates by comparing the size of the types array to the size of a Set created from types
  const hasDuplicates = types.length !== new Set(types).size;

  if (hasDuplicates) {
    alert('Each line must have a unique type. Please correct your selection.');
    return; // Stop execution if there are duplicates
  }

  // Check if the key (title) already exists in localStorage
  const key = title;
  const existingData = localStorage.getItem(key);

  if (existingData !== null) {
    // If the key already exists, ask the user for confirmation to overwrite
    const overwriteConfirmation = confirm('Data with the same title already exists. Do you want to overwrite it?');

    if (!overwriteConfirmation) {
      // If the user chooses not to overwrite, do nothing and return
      return;
    }
  }

  // Update typesOrder with the order of types in the current set
  typesOrder = types;

  // Proceed with saving the data if there are no duplicates or if the user confirms overwriting
  const dataToSave = {
    title: title,
    lyrics: lyrics,
    typesOrder: typesOrder,
  };

  const dataString = JSON.stringify(dataToSave);

  // Store the data in localStorage
  localStorage.setItem(key, dataString);

  // Call a function to clear the form
  clearForm();
}

  function clearForm() {
    title = '';
    lyrics = '';
    selectedSet = [];
    allLyrics = []; // Make sure to clear this as well
    // Reset or clear typesOrder as needed
  }
</script>

<style>
  .container {
    position: relative; /* Needed for absolute positioning of children */
    padding: 10px;
    box-sizing: border-box; /* Ensures padding is included in total width/height */
  }

  input[type="text"], textarea {
    width: 100%; /* Ensures alignment */
    margin-bottom: 10px; /* Space between elements */
    box-sizing: border-box; /* Ensures padding/border are included in total width/height */
  }

  textarea {
    height: 150px; /* Adjust height as needed */
    resize: none; /* Prevent resizing */
  }

  .buttons {
    display: flex;
    justify-content: space-between; /* Aligns buttons to left and right */
  }

  .hide-button {
    position: absolute;
    right: 10px; /* Align to the right edge of the container */
    bottom: 10px; /* Align to the bottom edge of the container */
    cursor: pointer;
  }
</style>

{#if isVisible}
  <div class="container" transition:slide>
    <input type="text" bind:value={title} placeholder="Title">
    <textarea bind:value={lyrics} placeholder="Paste lyrics here"></textarea>
    <div class="buttons">
      <button on:click={parseLyrics}>Parse</button>
      <!-- Parse button aligned to the left -->
    </div>
    <button class="hide-button" on:click={toggleVisibility}>Hide</button>
    <!-- Hide button positioned at the bottom-right -->
  </div>
{:else}
  <button on:click={toggleVisibility}>Show Lyrics Input</button>
{/if}

{#if selectedSet.length > 0}
  <div>
    <p>Select the type for each line:</p>
    {#each selectedSet as line, index (line.text)}
        <div>
            <label>{index + 1}: {line.text}</label> <!-- Corrected this line -->
            <select bind:value={line.type}>
            <option value="">Select Type</option>
            <option value="0">Original Japanese</option>
            <option value="1">Phonetic Korean</option>
            <option value="2">Korean Translation</option>
            </select>
        </div>
        {/each}

    <!-- Add a button to confirm selections -->
    <button on:click={confirmSelection}>Confirm</button>
  </div>
{/if}
