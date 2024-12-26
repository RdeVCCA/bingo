<!--
  - Button to roll 
  - Button to stop rolling (Probably timeout if left for too long) 
  - Button to reset history
-->

<script>
  // interface to accept bindings to buttons
  let { onPlay, onPause, onNewRound } = $props();
  let isPlaying = $state(false);

  function onKeyPress(event) {
    switch (event.key) {
      case " ":
        event.preventDefault();
        handlePlayPause();
        break;
    }
  }

  function handlePlayPause() {
    isPlaying = !isPlaying;
    if (!isPlaying) {
      onPause();
    } else {
      onPlay();
    }
  }
</script>

<style>
  .outer {
    flex-grow: 1;
    max-height: 20vh;
    display: flex;
    flex-direction: row;
    justify-content: center;
  }

  button {
    background-color: #eee;
    border-radius: 0;
    border: 2px solid black;
    aspect-ratio: 1/1;
    height: 100%;
  }

  button:hover {
    background-color: #ccc;
  }
</style>

<div class="outer">
  <!-- start/stop should be the same button -->
  <button onclick={handlePlayPause}>
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" style="display: {isPlaying ? 'none' : 'initial'}">
      <title>play</title>
      <path d="M8,5.14V19.14L19,12.14L8,5.14Z" />
    </svg>
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" style="display: {isPlaying ? 'initial' : 'none'}">
      <title>stop</title>
      <path d="M18,18H6V6H18V18Z" />
    </svg>
  </button>
  <button onclick={onNewRound}>
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
      <title>reload</title>
      <path d="M2 12C2 16.97 6.03 21 11 21C13.39 21 15.68 20.06 17.4 18.4L15.9 16.9C14.63 18.25 12.86 19 11 19C4.76 19 1.64 11.46 6.05 7.05C10.46 2.64 18 5.77 18 12H15L19 16H19.1L23 12H20C20 7.03 15.97 3 11 3C6.03 3 2 7.03 2 12Z" />
    </svg>
  </button>
</div>

<svelte:window onkeypress={onKeyPress} />
