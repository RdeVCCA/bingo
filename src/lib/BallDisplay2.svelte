<script>
  import { BingoCage } from "$lib/index.js";
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

<style>
  .outer {
    flex-grow: 8;
    align-self: stretch;
    display: flex;
    flex-direction: row;
    column-gap: 10%;
  }
</style>

<div class="outer">
  <div class="inner">
    <p class="display" in:fly={{ easing: quadIn, duration: 300, y: -400 }} out:fly={{ easing: quadOut, duration: 300, y: 400 }}>
      {letterFromNumber(number)} {number}
    </p>
  </div>
  <BingoCage {number}/>
</div>
