<script>
  let { unpicked } = $props();
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

  +-----+ \
  |thead| |
  +-----+ |
  |     | |
  |     | |
  |     | |
  |     | |> table
  |     | |
  |     | |
  |     | |
  |     | |
  |     | |
  +-----+ /
-->

<style>
  * {
    font-size: 20pt;
  }

  .outer {
    border: 2px solid black;
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(16, 1fr);
    height: 100%;
    aspect-ratio: 5/16;
  }

  .first-row {
    display: contents;
  }

  .first-row > div {
    background-color: #eee;    
    font-weight: bold;
  }

  div {
    border: 1px solid black;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: background-color 2.0s, color 2.0s;
  }

  .outer > div:not(.first-row) {
    color: transparent;
  }

  .outer > div:not(.first-row).filled {
    color: black;
  }

  div:nth-child(5n+2).filled {
    background-color: #ff9aa2;
  }

  div:nth-child(5n+3).filled {
    background-color: #ffdac1;
  }

  div:nth-child(5n+4).filled {
    background-color: #e2f0cb;
  }

  div:nth-child(5n).filled {
    background-color: #b5ead7;
  }

  div:nth-child(5n+1).filled {
    background-color: #c7ceea;
  }
</style>

<div class="outer">
  <div class="first-row">
    <div>B</div>
    <div>I</div>
    <div>N</div>
    <div>G</div>
    <div>O</div>
  </div>
  <!--
    Bingo balls each have unique numbers from 1-75.
    1-15 are labelled B
    16-30 are I
    31-45 are N
    46-60 are G
    61-75 are O
    thus we just need a bit of math to see which cell to check.
  -->
  {#each Array(15).fill() as _, i}
    <!-- 15 rows -->
    {#each Array(5).fill() as _, j}
      <!-- 5 columns -->
        <div class={unpicked.includes(j * 15 + i + 1) ? "" : "filled"}>
          {j * 15 + i + 1}
        </div>
    {/each}
  {/each}
</div>
