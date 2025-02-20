<script>
  import {
    BallDisplay,
    BallHistory,
    Controls,
    RoundProgress,
    BingoCage,
  } from "$lib/index.js";

  let bingoCage; // the BingoCage element will bind to this
  let soundEffect = new Audio("/soundEffect.mp3");
  soundEffect.loop = true;

  let appState = {
    unpicked: Array(75).fill().map((_, i) => i + 1),
    previewNumber: 0,
    actualNumber: -1,
    previewIntervalId: 0, // the previewNumbers are generated using a setInterval, this ensures we can stop the interval
  };
  
  /**
    * Returns a new ball. This function WILL NOT modify the `unpicked` array.
    * If specified, the function will ensure the new ball is not the same as `prevPicked`.
    */
  function previewNewBall(unpicked, prevPicked) {
    let newBallIndex = Math.floor(Math.random() * unpicked.length);
    if (unpicked.length > 1 && unpicked[newBallIndex] === prevPicked) {
      newBallIndex += 1;
      if (newBallIndex >= unpicked.length) {
        newBallIndex -= 2;
      }
    }
    return unpicked[newBallIndex];
  }

  /** Returns a new ball. This function WILL modify the `unpicked` array. */
  function pickNewBall(unpicked) {
    let newBallIndex = Math.floor(Math.random() * unpicked.length);
    let newBall = unpicked.splice(newBallIndex, 1)[0];
    return newBall;
  }

  function onPlay() {
    if (appState.unpicked.length === 0) { return; }
    soundEffect.play();

    appState.actualNumber = -1;
    appState.previewNumber = previewNewBall(appState.unpicked, appState.previewNumber);
    appState.previewIntervalId = setInterval(function () {
      appState.previewNumber = previewNewBall(appState.unpicked, appState.previewNumber);
    }, 300);
  }

  function onPause() {
    if (appState.unpicked.length === 0) { return; }

    soundEffect.pause();
    soundEffect.currentTime = 0;

    clearInterval(appState.previewIntervalId);

    appState.previewNumber = -1;
    appState.actualNumber = pickNewBall(appState.unpicked);
    bingoCage.removeBall();
  }

  function onNewRound() {
    appState.unpicked = Array(75).fill().map((_, i) => i + 1);
    appState.previewNumber = 0;
    appState.actualNumber = -1;
    bingoCage.resetBalls();
  }
</script>

<!--
  +--< outer >---------------------+------+
  |+-< inner >--------+-----------+|      |
  ||                  |           ||      |
  ||                  |           ||      |
  ||    BallDisplay   | BingoCage ||   <----- previous balls
  ||                  |           ||      |   (BallDisplay.svelte)
  |+------------------+-----------+|      |
  ||      progress indicator      ||      |
  |+------------------------------+|      |
  ||  controls (Controls.svelte)  ||      |
  |+------------------------------+|      |
  +--------------------------------+------+
-->

<style>
  .outer {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    column-gap: 8px;
  }

  .inner {
    flex-grow: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    row-gap: 8px;
  }

  .display {
    align-self: stretch;
    flex-grow: 3;
    display: flex;
    flex-direction: row;
    column-gap: 20px;
    justify-content: space-around;
  }
</style>

<div class="outer">
  <div class="inner">
    <div class="display">
      <BallDisplay previewNumber={appState.previewNumber} actualNumber={appState.actualNumber} />
      <BingoCage bind:this={bingoCage} />
    </div>
    <RoundProgress rounds={75 - appState.unpicked.length} />
    <Controls {onPlay} {onPause} {onNewRound} roundEnded={appState.unpicked.length === 0}/>
  </div>
  <BallHistory unpicked={appState.unpicked} />
</div>

