<!--
  - Button to roll 
  - Button to stop rolling (Probably timeout if left for too long) 
  - Button to reset history
-->

<script>
  // interface to accept bindings to buttons
  let { onPlay, onPause, onNewRound, roundEnded } = $props();
  let isPlaying = $state(false);

  function onKeyPress(event) {
    switch (event.key) {
      case " ":
        /*
          blur() function is to remove focus from the button;
          without it the spacebar will trigger the button when pressed (default behaviour)
          no I don't know why the function to unfocus is called blur()
          
          oh wait
        */
        document.activeElement.blur();
        handlePlayPause();
        event.preventDefault();
        break;
    }
  }

  function handlePlayPause() {
    if (roundEnded) { return; }
    isPlaying = !isPlaying;
    if (!isPlaying) {
      onPause();
    } else {
      onPlay();
    }
  }
</script>

<!--
  +--< outer >---------------------+------+
  |+-< inner >--------------------+|      |
  ||                              ||      |
  ||     ball number display      ||      |
  ||     (BallDisplay.svelte)     ||   <----- previous balls
  ||                              ||      |   (BallDisplay.svelte)
  |+------------------------------+|      |
  ||      progress indicator      ||      |
  |+------------------------------+|      |
  ||  controls (Controls.svelte)  ||      |
  |+------------------------------+|      |
  +--------------------------------+------+

  +--< outer >----------------------------+
  |            +-----+ +-----+            |
  |            | but | | but |            |
  |            | ton | | ton |            |
  |            +-----+ +-----+            |
  +---------------------------------------+
-->

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

  button[disabled] {
    filter: contrast(50%);
  }

  button:not([disabled]):hover {
    background-color: #ccc;
  }
</style>

<div class="outer">
  <!-- start/stop should be the same button -->
  <button onclick={handlePlayPause} disabled="{roundEnded}" aria-label="Next number" title="Next number">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" style="display: {isPlaying ? 'none' : 'initial'}" aria-hidden="true">
      <path d="M8,5.14V19.14L19,12.14L8,5.14Z" />
    </svg>
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" style="display: {isPlaying ? 'initial' : 'none'}" aria-hidden="true">
      <path d="M18,18H6V6H18V18Z" />
    </svg>
  </button>
  <button onclick={onNewRound} aria-label="New round" title="New round">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" aria-hidden="true">
      <path d="M2 12C2 16.97 6.03 21 11 21C13.39 21 15.68 20.06 17.4 18.4L15.9 16.9C14.63 18.25 12.86 19 11 19C4.76 19 1.64 11.46 6.05 7.05C10.46 2.64 18 5.77 18 12H15L19 16H19.1L23 12H20C20 7.03 15.97 3 11 3C6.03 3 2 7.03 2 12Z" />
    </svg>
  </button>
</div>

<svelte:window onkeypress={onKeyPress} />
