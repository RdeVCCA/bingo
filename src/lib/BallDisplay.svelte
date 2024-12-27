<script>
  import { fly } from "$lib/transition.js";
  import { quadIn, quadOut } from "svelte/easing";
  let { number } = $props();

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

  +--< outer > ---------------------------+
  |+-< display; position: absolute >-----+|
  ||                                     ||
  ||                                     ||
  ||                                     ||
  |+-------------------------------------+|
  +---------------------------------------+
-->

<style>
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
    font-size: 30vw; /* vw to make it big and scale responsively */
  }

  .outer {
    position: relative;
    align-self: stretch;
    flex-grow: 8;
  }
</style>

<div class="outer">
  {#key number}
    <p class="display" in:fly={{ easing: quadIn, duration: 300, y: -400 }} out:fly={{ easing: quadOut, duration: 300, y: 400 }}>
      {letterFromNumber(number)} {number}
    </p>
  {/key}
</div>
