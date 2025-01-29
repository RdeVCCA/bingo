<script>
  import { BingoCage } from "$lib/index.js";
  import { fly } from "$lib/transition.js";
  import { quadIn, quadOut } from "svelte/easing";
  let { previewNumber } = $props();

  function letterFromNumber(number) {
    if (number >= 1 && number <= 15) {
      return "B";
    } else if (number >= 16 && number <= 30) {
      return "I";
    } else if (number >= 31 && number <= 45) {
      return "N";
    } else if (number >= 46 && number <= 60) {
      return "G";
    } else if (number >= 61 && number <= 75) {
      return "O";
    } else {
      console.error(`letterFromNumber: number ${number} is not in the range 1-75`);
    }
  }

  // let letterToColor = {
  //   "B": "#660008",
  //   "I": "#702d00",
  //   "N": "#3f5619",
  //   "G": "#17513c",
  //   "O": "#1e274f",
  // };
  let letterToColor = {
    "B": "#ff9aa2",
    "I": "#ffdac1",
    "N": "#e2f0cb",
    "G": "#b5ead7",
    "O": "#c7ceea",
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

  +--< display-container >--------------+
  |+-< display; position: absolute >---+|
  ||                                   ||
  ||                                   ||
  ||                                   ||
  |+-----------------------------------+|
  +-------------------------------------+
-->

<style>
  .display-container {
    position: relative;
    flex-shrink: 0;
    width: 60%;
  }

  .display {
    color: #222;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
    text-align: center;
    z-index: -5; /* make sure the rolling numbers don't draw on top of any UI */
    margin: 0;
    display: inline-block;
    font-size: 15vw; /* vw to make it big and scale responsively */
  }

  span {
    font-family: monospace;
  }
</style>

<div class="display-container">
  {#key previewNumber}
    <p class="display" in:fly={{ easing: quadIn, duration: 300, y: -400 }} out:fly={{ easing: quadOut, duration: 300, y: 400 }}>
      <span style="background-color: {letterToColor[letterFromNumber(previewNumber)]}">
        {letterFromNumber(previewNumber)}
      </span>
      <span>
        {previewNumber}
      </span>
    </p>
  {/key}
</div>
