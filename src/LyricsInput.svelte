<script>
  import { slide } from 'svelte/transition';
  let title = '';
  let lyrics = '';
  let isVisible = true;

  function toggleVisibility() {
    isVisible = !isVisible;
  }

  function parseLyrics() {
    console.log({ title, lyrics });
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
