<script>
  import ControlBar from './ControlBar.svelte';
</script>

<style>
  .container {
    display: flex;
    height: 100vh;
  }

  .sidebar {
    width: 200px;
    background-color: #f0f0f0;
  }

  .main-content {
    flex-grow: 1;
    background-color: #fff;
    padding: 20px;
    display: flex;
    flex-wrap: wrap;
    gap: 20px; /* Space between placeholder boxes */
  }

  .placeholder-box {
    width: 100px;
    height: 100px;
    background-color: #d3d3d3;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #333;
  }
</style>

<div class="container">
  <div class="sidebar">
    <ControlBar />
  </div>
  <div class="main-content">
    <!-- Placeholder boxes in the main content area -->
    <div class="placeholder-box">Content 1</div>
    <div class="placeholder-box">Content 2</div>
    <div class="placeholder-box">Content 3</div>
    <!-- Add more placeholders as needed -->
  </div>
</div>
