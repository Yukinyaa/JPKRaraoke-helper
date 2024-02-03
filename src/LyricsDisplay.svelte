<!-- LyricsDisplay.svelte -->
<script>
  export let lyricsSet = {
    jp: '',
    krPhonics: '',
    krTranslation: '',
  };

  export let opacity = false;
  export let fontSizePx = 24;
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
</style>

<div class="lyrics-container" style="opacity: {opacity}">
  <p class="kr-translation" style="font-size: {fontSizePx * 0.3}px">{lyricsSet.krTranslation}</p>
  <p class="kr-phonics"     style="font-size: {fontSizePx}px">{lyricsSet.krPhonics}</p>
  <p class="jp-line"        style="font-size: {fontSizePx * 0.8}px">{lyricsSet.jp}</p>
</div>
