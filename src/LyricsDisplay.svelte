<!-- LyricsDisplay.svelte -->
<script>
  export let lyricsSet = {
    jp: '',
    krPhonics: '',
    krTranslation: '',
  };

  export let current = false;

  // Calculate opacity based on the "current" prop
  let opacity = current ? 1 : 0.6;
</script>

<style>
  .lyrics-container {
    /* Your existing styles */
    background-color: #f0f0f0;
    border: 2px solid #333;
    border-radius: 10px;
    max-width: 80%;
    padding: 10px;
    text-align: center;
  }

  .jp-line {
    font-size: 24px; /* Adjust font size as needed */
    font-weight: bold;
  }

  .kr-phonics {
    font-size: 16px; /* Adjust font size as needed */
  }

  .kr-translation {
    font-size: 16px; /* Adjust font size as needed */
  }
</style>

<div class="lyrics-container" style="opacity: {opacity}">
  <p class="kr-translation">{lyricsSet.krTranslation}</p>
  <p class="jp-line">{lyricsSet.jp}</p>
  <p class="kr-phonics">{lyricsSet.krPhonics}</p>
</div>
