<script>
  // ControlBar logic
</script>

<style>
  .control-bar {
    padding: 20px;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  button {
    padding: 10px 20px;
    cursor: pointer;
  }

  .placeholder-box {
    height: 50px;
    background-color: #d3d3d3; /* Light gray */
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #333;
  }
</style>

<div class="control-bar">
  <button on:click={() => console.log('Previous')}>Previous</button>
  <button on:click={() => console.log('Next')}>Next</button>
  <!-- Placeholder boxes -->
  <div class="placeholder-box">Box 1</div>
  <div class="placeholder-box">Box 2</div>
</div>
