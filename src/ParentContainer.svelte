<!-- ParentContainer.svelte -->
<script>
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';
  import LyricsDisplay from './LyricsDisplay.svelte';

  let lyricsSets = [
    {
      jp: 'Japanese 1',
      krPhonics: 'Korean Phonics 1',
      krTranslation: 'Korean Translation 1',
    },
    {
      jp: 'Japanese 2',
      krPhonics: 'Korean Phonics 2',
      krTranslation: 'Korean Translation 2',
    },
    // Add more lyrics sets here
  ];

  let currentIndex = writable(0);

  onMount(() => {
    // Implement logic to scroll and load new lyrics sets here
  });
</script>

<style>
  /* Styles for the parent container and animation */
  .lyrics-display-container {
    transition: transform 0.5s;
  }
</style>

<div class="parent-container">
  {#each lyricsSets as lyricsSet, index}
    <div class="lyrics-display-container">
      <LyricsDisplay
        {lyricsSet}
        current={index === $currentIndex}
      />
    </div>
  {/each}
</div>
