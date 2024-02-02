<script>
  import ControlBar from './ControlBar.svelte';
  import LyricsInput from './LyricsInput.svelte';
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
  <div class="lyrics-input-container">
    <LyricsInput />
  </div>
</main>