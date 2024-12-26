<!--
  +--< outer >----------------------------+
  |+-< inner >--------------------+       |
  ||                              |       |
  ||     ball number display      |       |
  ||     (BallDisplay.svelte)     |   <------ previous balls
  ||                              |       |   (BallDisplay.svelte)
  |+------------------------------+       |
  ||  controls (Controls.svelte)  |       |
  |+------------------------------+       |
  +---------------------------------------+
-->

<script>
  import {
    BallDisplay,
    BallHistory,
    Controls,
  } from "$lib/index.js";

  /** Returns a new ball. This function WILL NOT modify the `unpicked` array */
  function previewNewBall(unpicked) {
    let newBallIndex = Math.floor(Math.random() * unpicked.length);
    return unpicked[newBallIndex];
  }

  /** Returns a new ball. This function WILL modify the `unpicked` array */
  function pickNewBall(unpicked) {
    let newBallIndex = Math.floor(Math.random() * unpicked.length);
    let newBall = unpicked.splice(newBallIndex, 1)[0];
    return newBall;
  }
  
  let unpicked = $state(Array(75).fill().map((_, i) => i + 1));
  let previewNumber = $state(1);
  let soundEffect = new Audio("/saul.mp3");
  soundEffect.loop = true;

  let previewIntervalId;
  function onPlay() {
    if (unpicked.length === 0) { return; }
    soundEffect.play();
    previewIntervalId = setInterval(function () {
      previewNumber = previewNewBall(unpicked);
    }, 50);
  }

  function onPause() {
    if (unpicked.length === 0) { return; }
    soundEffect.pause();
    soundEffect.currentTime = 0;
    clearInterval(previewIntervalId);
    previewNumber = pickNewBall(unpicked);
  }

  function onNewRound() {
    unpicked = Array(75).fill().map((_, i) => i + 1);
    previewNumber = 1;
  }

</script>

<style>
  .outer {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    flex-grow: 1;
  }

  .inner {
    flex-grow: 5;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
  }
</style>

<div class="outer">
  <div class="inner">
    <BallDisplay number={previewNumber} />
    <Controls {onPlay} {onPause} {onNewRound} />
  </div>
  <BallHistory unpicked={unpicked}/>
</div>

