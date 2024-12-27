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
    transition: background-color 0.5s;
  }
  
  div.filled {
    background-color: #666;
    color: white;
  }

  .outer {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(16, 1fr);
    height: 100%;
    aspect-ratio: 5/16;
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
        <!-- HACK: not sure why the table columns are shrinking so fill empty cells with &emsp to pad them up -->
        <div class={unpicked.includes(j * 15 + i + 1) ? "" : "filled"}>
          {@html unpicked.includes(j * 15 + i + 1) ? "&emsp;" : j * 15 + i + 1}
        </div>
    {/each}
  {/each}
</div>
