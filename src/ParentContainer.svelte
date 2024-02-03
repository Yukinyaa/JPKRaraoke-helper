<!-- ParentContainer.svelte -->
<script>
  import LyricsDisplay from "./LyricsDisplay.svelte";

  export let loadedData = null;

  // Extract properties from loadedData
  let { title, lyrics, typesOrder } = loadedData;

  // Calculate indices for different types based on typesOrder
  let jpNo = typesOrder.indexOf('0');
  let phNo = typesOrder.indexOf('1');
  let krNo = typesOrder.indexOf('2');

  let currentLine = 0; // Initialize currentLine
  let linesToShow = 5; // Number of lines to show including the current line

  // Cache the split lyrics for performance
  let splitLyrics = lyrics.split('\n').filter(line => line.trim() !== '');

  // Function to calculate start and end values based on currentLine
  function calculateRange(currentLine) {
    const start = Math.max(0, currentLine - Math.floor(linesToShow / 2));
    const end = Math.min(splitLyrics.length, start + linesToShow);
    return { start, end };
  }

  // Function to handle key presses and increment/decrement currentLine
  function handleKeyPress(event) {
    if (event.key === 'ArrowDown' || event.key === 'ArrowRight' || event.key === ' ') {
      if (currentLine < splitLyrics.length - 1) {
        currentLine++;
      }
    } else if (event.key === 'ArrowUp' || event.key === 'ArrowLeft') {
      if (currentLine > 0) {
        currentLine--;
      }
    }
  }

  // Function to handle touch and click events for navigation
  function handleInteraction(event) {
    const interactionY = event instanceof TouchEvent ? event.touches[0].clientY : event.clientY;

    // Determine if the interaction is in the topmost lyrics bar
    const topLyricsBar = document.querySelector('.lyrics-display-container:first-child');
    const topLyricsBarRect = topLyricsBar.getBoundingClientRect();

    if (interactionY < topLyricsBarRect.bottom) {
      // Decrease currentLine if touched/clicked in the top bar
      if (currentLine > 0) {
        currentLine--;
      }
    } else {
      // Increase currentLine for other touch/click areas
      if (currentLine < splitLyrics.length - 1) {
        currentLine++;
      }
    }
  }

  // Listen for keydown events to handle key presses
  window.addEventListener('keydown', handleKeyPress);

  // Listen for touch events and click events to handle interaction
  document.addEventListener('touchstart', handleInteraction);
  document.addEventListener('click', handleInteraction);

  // Ensure the currentLine stays within bounds
  $: currentLine = Math.max(0, Math.min(currentLine, splitLyrics.length - 1));
</script>

<style>
  /* Styles for the parent container and animation */
  .lyrics-display-container {
    transition: transform 0.5s;
  }
</style>

<div class="parent-container">
  {#each [-1, 0, 1, 2, 3] as index}
    <div class="lyrics-display-container">
      <!-- Pass the currentLine to LyricsDisplay to display the corresponding lyrics -->
      <LyricsDisplay
        lyricsSet = {{
          jp: jpNo >= 0 ? splitLyrics[(currentLine + index) * 3 + jpNo] : '', // Check if jpNo is valid
          krPhonics: phNo >= 0 ? splitLyrics[(currentLine + index) * 3 + phNo] : '', // Check if phNo is valid
          krTranslation: krNo >= 0 ? splitLyrics[(currentLine + index) * 3 + krNo] : '', // Check if krNo is valid
        }}
        opacity = {1 - Math.abs(index) * 0.2}
        fontSizePx =  {24 * (1 - Math.abs(index) * 0.2)}
      />
    </div>
  {/each}
</div>
