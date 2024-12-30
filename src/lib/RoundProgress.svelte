<script>
  let { rounds } = $props();
  let progress = $derived(`${rounds}/75`);
  let percentage = $derived((rounds / 75) * 100);
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

  +--< outer >----------------------------+
  |+-< inner >----+ <--- (width styled    |
  |+--------------+      dynamically)     |
  +---------------------------------------+
-->

<style>
  .outer {
    background-color: white;
    border: 2px solid black;
    width: 100%;
    height: fit-content;
    position: relative;
  }

  .label {
    z-index: 2;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    text-align: center;
  }

  .inner {
    border-right: 2px solid #aaa;
    background-color: #eee;
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    z-index: 1;
  }
</style>

<div class="outer">
  &emsp; <!-- HACK: empty space to prop up the `outer` div -->
  <div class="label">{progress}</div>
  <div class="inner" style="width: {percentage}%"></div>
</div>
