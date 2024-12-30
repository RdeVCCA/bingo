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
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
    text-align: center;
    z-index: -5; /* make sure the rolling numbers don't draw on top of any UI */
    margin: 0;
    display: inline-block;
    font-size: 20vw; /* vw to make it big and scale responsively */
  }

</style>

<div class="display-container">
  {#key previewNumber}
    <p class="display" in:fly={{ easing: quadIn, duration: 300, y: -400 }} out:fly={{ easing: quadOut, duration: 300, y: 400 }}>
      {letterFromNumber(previewNumber)} {previewNumber}
    </p>
  {/key}
</div>
