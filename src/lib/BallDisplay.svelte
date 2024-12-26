<script>
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
  +-----------+-----------+
  |           |           |
  |           |           |
  |  letter   |  numbers  |
  |           |           |
  |           |           |
  +-----------+-----------+
-->

<style>
  .outer {
    display: flex;
    flex-direction: row;
    justify-content: center;
    column-gap: 10%;
  }

  .letter, .number {
    font-size: 30vw; /* vw to make it big and scale responsively */
  }
</style>

<div class="outer">
  <div class="letter">
    {letterFromNumber(number)}
  </div>
  <div class="number">
    {number}
  </div>
</div>
