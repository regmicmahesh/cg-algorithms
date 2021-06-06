<script lang="ts">
  import { onMount } from "svelte";

  let canvas: HTMLCanvasElement;
  let context: CanvasRenderingContext2D;
  let x1: number, x2: number, y1: number, y2: number;

  let dx = 0;
  let dy = 0;
  let steps = 0;
  let xinc = 0;
  let yinc = 0;

  let cWidth = 800;
  let cHeight = 600;

  onMount(() => {
    context = canvas.getContext("2d");
    context.lineWidth = 2;

    context.moveTo(0, 0);
  });

  const drawLine = () => {
    let x = x1;
    let y = y1;
    dx = x2 - x1;
    dy = y2 - y1;
    if (dx > dy) {
      steps = Math.abs(dx);
    } else {
      steps = Math.abs(dy);
    }
    xinc = dx / steps;
    yinc = dy / steps;

    context.fillStyle = "black";

    for (let v = 0; v < steps; v++) {
      x = x + xinc;
      y = y + yinc;
      setTimeout(putPixel.bind(undefined, x, y), (v + 1) * 50);
    }
  };
  const putPixel = (x: number, y: number) => {
    console.log(x, y);
    context.fillRect(x, y, 1, 1);
  };
</script>

<main>
  <canvas bind:this={canvas} width={cWidth} height={cHeight} />
  <div>
    <h1>DDA Visualizer</h1>
    Resolution: {cHeight}x{cWidth}

    <hr />
    Point One:
    <input type="number" bind:value={x1} />
    <input type="number" bind:value={y1} />

    <br />
    Point Two:
    <input type="number" bind:value={x2} />
    <input type="number" bind:value={y2} />
    <br />

    <button on:click={drawLine}>Draw Line</button>
    <hr />
    <h3>dx: {dx}</h3>
    <h3>dy: {dy}</h3>
    <h3>Steps: {steps}</h3>
  </div>
</main>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  canvas {
    border: 2px solid black;
  }
  main {
    display: flex;
    height: 90vh;
    overflow: hidden;
    gap: 30px;
  }
  div {
    flex-grow: 1;
  }
</style>
