<script lang="ts">
  import { onMount } from "svelte";

  let canvas: HTMLCanvasElement;
  let context: CanvasRenderingContext2D;
  let x1: number, x2: number, y1: number, y2: number;
  let speed: number = 1;

  let dx = 0;
  let dy = 0;
  let steps = 0;
  let xinc = 0;
  let yinc = 0;

  let currX = 0;
  let currY = 0;

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
      setTimeout(putPixel.bind(undefined, x, y), ((v + 1) * 50) / speed);
    }
  };
  const putPixel = (x: number, y: number) => {
    currX = x;
    currY = y;
    console.log(x, y);
    context.fillRect(x, cHeight - y, 1, 1);
  };
</script>

<main>
  <div class="nav-bar">
    <a href="#">BFA visulizer</a>
    <a href="#">DFF visulizer</a>
  </div>

  <canvas bind:this={canvas} width={cWidth} height={cHeight} />
  <div class="visualizer">
    <h1>DDA Visualizer</h1>
    <div>
      Resolution: {cHeight}x{cWidth}
      <input type="number" bind:value={cWidth} />
      <input type="number" bind:value={cHeight} />
    </div>

    <hr />
    <div>
      Point One:
      <input type="number" bind:value={x1} />
      <input type="number" bind:value={y1} />
    </div>
    
    <div class="axis">
      <p>X <sub>1</sub></p>
      <p>X <sub>2</sub></p>
    </div>

    <br />
    <div>
      Point Two:
      <input type="number" bind:value={x2} />
      <input type="number" bind:value={y2} />
    </div>
    
   

    <div class="axis">
      <p>Y <sub>1</sub></p>
      <p>Y <sub>2</sub></p>
    </div>
    <br />

    <button on:click={drawLine}>Draw Line</button>
    <hr />
  </div>
  <div class="coordinate-info">
    <h3 class="dx">dx: {dx}</h3>
    <h3 class="dy">dy: {dy}</h3>
    <h3 class="steps">Steps: {steps}</h3>
    <h3 class="currentX">Current X: {currX}</h3>
    <h3 class="currentY">Current Y: {currY}</h3>
    <h3 class="speed">
      Speed <input
        bind:value={speed}
        type="range"
        min="0.5"
        max="5"
        step="0.5"
      />
    </h3>
  </div>
</main>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Source+Sans+Pro:wght@200;400&display=swap");
      * {
        font-family: "Source Sans Pro", sans-serif;
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }

      main {
        display: grid;
        height: 90vh;
        overflow: hidden;
        grid-template-columns: 2fr 1fr;
        grid-template-areas:
          " nav-bar nav-bar"
          "canvas visualizer"
          "canvas coordinate-info";
        grid-gap: 5px;
        background: black;
        color: white;
      }

      /* nav bar */
      .nav-bar {
        grid-area: nav-bar;
        border-bottom: 1px solid black;
        background: rgba(38, 187, 197, 0.4);
        display: flex;
        justify-content: flex-end;
        align-items: center;
        height: 4em;
      }
      .nav-bar a {
        text-decoration: none;
        margin: 0 10px;
        padding: 10px;
        background: white;
        border-radius: 10px;
        border: 2px solid black;
        color: black;
      }

      .nav-bar a:hover {
        background: black;
        color: white;
        border: 2px solid white;
      }
      .nav-bar a:active {
        box-shadow: 0 2px #666;
        transform: translateY(4px);
      }

      canvas {
       border: 2px solid white;
       background: teal;
       width: 100%;
       height: 100%;
       grid-area: canvas;
      }

      .visualizer {
        grid-area: visualizer;
        text-align: center;
      }

      .visualizer div {
        display: flex;
        flex-direction: row;
        align-items: baseline;
        justify-content: space-around;
        
      }
      .axis{
        display: flex;
        margin-left: 130px;
      }

      input[type="number"] {
        color: white;
        margin: 5px;
        border: none;
        background: transparent;
        text-align: center;
        border: solid thin rgba(70, 116, 255, 0.7);
        border-radius: 10px;
        outline: none;
        width: 25%;
        height: 2em;
        font-size: 20px;
      }

      button {
        margin-left: 100px;
        margin-bottom: 20px;
        padding: 10px;
        background: white;
        border-radius: 10px;
        border: 2px solid aqua;
        color: black;
        outline: none;
      }
      button:active {
        box-shadow: 0 2px #666;
        transform: translateY(4px);
      }
      button:hover {
        background: black;
        color: white;
        border: 2px solid white;
      }
      .coordinate-info{
        grid-area: coordinate-info;
        margin-bottom: 150px;
        margin-left: 20px;
        display: grid;
        grid-template-areas: "dx  dy"
        "steps ."
        "currentX currentY"
        "speed .";
      }
      .dx{
        grid-area: dx;
      }
      .dy{
        grid-area: dy;
      }
      .steps{
        grid-area: steps;
        
      }
      .currentX{
        grid-area: currentX;
      }
      .currentY{
        grid-area: currentY;
      }
      .speed{
        grid-area: speed;
      }
</style>
