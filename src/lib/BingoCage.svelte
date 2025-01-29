<script>
  let lastBallDistance = [];
  function generateRandomPosition() {
    let selfRadius = 32;
    let minRadius = 100;
    let maxRadius = 240;
    let layerConstant = 0.1;
    let layerThickness = (maxRadius - minRadius) / 10;

    let trueMinRadius = minRadius + selfRadius;
    let trueMaxRadius = maxRadius - selfRadius;

    // decide how far away the balls are from the center
    // we generate the balls in circular layers, with the number
    // of balls in each layer proportional to the circumference
    // of that layer, which is in turn proportional to the radius of that layer.
    // the first layer to be filled is the outermost one.
    if (lastBallDistance.length === 0 || lastBallDistance[0].count === lastBallDistance[0].target) {
      let distance;
      if (lastBallDistance.length === 0) {
        distance = trueMaxRadius;
      } else {
        distance = lastBallDistance[0].distance - layerThickness;
      }
      lastBallDistance.unshift({ distance, count: 0, target: Math.floor(trueMaxRadius * layerConstant) });
    }

    // random stagger the distances but only if their distance isn't the maximum
    let distance = lastBallDistance[0].distance;
    if (distance !== trueMaxRadius) {
      distance += -5 + 10 * Math.random();
    }
    lastBallDistance[0].count += 1;
    
    // have a random angle displacement for balls of +-15 degrees
    let angleDisplacement = 15 * Math.random();

    // stagger animations randomly, max 3 second stagger
    // the stagger timing is always negative, because
    // we want a negative animation-delay value
    // https://css-irl.info/quick-tip-negative-animation-delay/
    let stagger = -3000 * Math.random();

    return { angleDisplacement, distance, stagger };
  }

  let colors = ["#ff9aa2", "#ffb7b2", "#ffdac1", "#e2f0cb", "#b5ead7", "#c7ceea"]

  // sort positions so balls further away from the edge are removed first
  let positions = Array(75).fill().map(_ => generateRandomPosition()).sort((a, b) => a.distance - b.distance);

  export function removeBall() {
    let ball = document.querySelector(".ball:not(.about-to-drop)");
    ball.classList.add("about-to-drop");
    let onIter;
    onIter = () => {
      ball.classList.remove("ball", 'about-to-drop');
      ball.classList.add("dropped-ball");
      ball.removeEventListener("animationiteration", onIter);
    };
    ball.addEventListener("animationiteration", onIter);
  }

  export function resetBalls() {
    for (let ball of document.querySelectorAll(".dropped-ball")) {
      ball.classList.remove("dropped-ball");
      ball.classList.add("ball");
    }
  }
</script>

<style>
  svg {
    min-width: 0;
    min-height: 0;
  }

  @keyframes infiniteSpin {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
  }

  .cage {
    animation: infiniteSpin 1.7s infinite linear;
    transform-origin: 50% 50%;
    transform-box: fill-box;
  }

  @keyframes ballAnim {
    0% {
      transform: rotate(90deg) translate(var(--ball-distance), 0);
    }
    70% {
      transform: rotate(190deg) translate(var(--ball-distance), 0);
    }
    100% {
      transform: rotate(90deg) translate(var(--ball-distance), 0);
    }
  }

  .ball {
    /*
      how to arrive at 0.793650s:
      the formula is (angle span of ball animation / angle span of cage animation in 1s) * (100% / percentage of that keyframe)
      so (100deg / 180deg) * (100% / 70%)
      = 50/63 ~= 0.793650s
    */
    animation: ballAnim 0.793650s infinite linear;
    animation-delay: var(--ball-anim-stagger);
  }
</style>

<!-- generated from https://svgomg.net/ then tweaked slightly; original file included in source -->
<svg viewBox="-264.9 -238.6 592.2 600.6" xmlns="http://www.w3.org/2000/svg" class="svg">
  <!-- this is the prototype ball, it's hidden behind the center of the cage -->
  <g stroke="#333" stroke-width="2" stroke-linecap="round">
    {#each Array(75).fill() as _, i}
      <!-- distance is a custom attribute and means nothing in SVG -->
      <circle r="30" class="ball"
        style="--ball-id: {i}; --ball-anim-stagger: {positions[i].stagger}ms; --ball-distance: {positions[i].distance}px; fill: {colors[i % 6]}"
        distance={positions[i].distance}
      />
    {/each}
  </g>
  <g stroke-linecap="round">
    <g stroke="#444" stroke-width="5" class="cage">
      <circle r="237.1" fill="none" />
      <circle r="197.6" fill="none" />
      <circle r="158.1" fill="none" />
      <circle r="118.6" fill="none" />
      <circle r="79" fill="none" />
      <circle r="39.5" />
      <path d="M0 237.1V-237M-233.5 41.2l467-82.4M-233.5-41.2l467 82.4" />
    </g>
    <path d="m0 0-130 357M0 0l130 357M-259.9 357H260M-259.9 357l422.1-103.6c60-13 75-37.7 103.9-80.1C294.9 130.8 314 3.9 314 3.9" fill="none" stroke="#333" stroke-width="10" />
  </g>
</svg>
