<script>
  import {
    BallDisplay,
    BallHistory,
    Controls,
    RoundProgress,
    BingoCage,
  } from "$lib/index.js";

  /**
    * Returns a new ball. This function WILL NOT modify the `unpicked` array.
    * If specified, the function will ensure the new ball is not the same as `prevPicked`.
    */
  function previewNewBall(unpicked, prevPicked) {
    let newBallIndex = Math.floor(Math.random() * unpicked.length);
    if (prevPicked) {
      while (unpicked.length > 1 && unpicked[newBallIndex] === prevPicked) {
        newBallIndex = Math.floor(Math.random() * unpicked.length);
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

  // generate array with numbers from 1-75
  let unpicked = $state(Array(75).fill().map((_, i) => i + 1));
  // the bingo cage svg binds to this variable
  let bingoCage;

  let previewNumber = $state(1);
  let previewIntervalId;
  let soundEffect = new Audio("/soundEffect.mp3");
  soundEffect.loop = true;
  function onPlay() {
    if (unpicked.length === 0) { return; }
    soundEffect.play();

    previewNumber = previewNewBall(unpicked, previewNumber);
    previewIntervalId = setInterval(function () {
      previewNumber = previewNewBall(unpicked, previewNumber);
    }, 300);
  }

  function onPause() {
    if (unpicked.length === 0) { return; }
    soundEffect.pause();
    soundEffect.currentTime = 0;
    clearInterval(previewIntervalId);
    previewNumber = pickNewBall(unpicked);
    bingoCage.removeBall();
  }

  function onNewRound() {
    unpicked = Array(75).fill().map((_, i) => i + 1);
    previewNumber = 1;
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
      <BallDisplay {previewNumber} />
      <BingoCage bind:this={bingoCage} />
    </div>
    <RoundProgress rounds={75 - unpicked.length} />
    <Controls {onPlay} {onPause} {onNewRound} roundEnded={unpicked.length === 0}/>
  </div>
  <BallHistory {unpicked} />
</div>

