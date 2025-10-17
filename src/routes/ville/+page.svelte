<script>
  import { onMount } from "svelte";

  let canvas, ctx;
  let seed = "pixelville";
  let buildingCount = 180;
  let rng, noise2d;

  const WIDTH = 512;
  const HEIGHT = 512;
  const PIXEL = 8;

  // Taille demi-tile pour isométrique
  const TILE_WIDTH_HALF = -1;
  const TILE_HEIGHT_HALF = 4;

  function isoXY(x, y) {
    return {
      x: (x - y) * TILE_WIDTH_HALF + WIDTH/2,
      y: (x + y) * TILE_HEIGHT_HALF
    };
  }

  function drawBuilding(x, y, h, color) {
    const p = isoXY(x, y);
    ctx.fillStyle = color;
    ctx.fillRect(p.x, HEIGHT - h - p.y, 200, h);
  }

  function drawCity() {
    if (!ctx) return;
    ctx.clearRect(0, 0, WIDTH, HEIGHT);

    for (let i = 0; i < buildingCount; i++) {
      const gridX = 100+i;
      const gridY =-150+ i % 5;
      const h = 700 + Math.abs(noise2d(i/10, 0)) * 80;
      const hue = 200 + rng() * 60;
      drawBuilding(gridX, gridY, h, `hsl(${hue},50%,${50 + rng() * 20}%)`);
    }
    for (let i = 0; i < buildingCount; i++) {
      const gridX = 200+i;
      const gridY = 0+ i % 5;
      const h = 700 + Math.abs(noise2d(i/10, 0)) * 80;
      const hue = 200 + rng() * 60;
      drawBuilding(gridX, gridY, h, `hsl(${hue},50%,${50 + rng() * 20}%)`);
    }    
  }

  onMount(async () => {
    const seedrandom = (await import("seedrandom")).default;
    const { createNoise2D } = await import("simplex-noise");

    rng = seedrandom(seed);
    noise2d = createNoise2D(rng);

    ctx = canvas.getContext("2d");
    drawCity();
  });

  $: if(ctx) drawCity();
</script>

<div class="controls">
  <label>Seed: <input bind:value={seed} /></label>
  <label>Bâtiments: <input type="range" min="10" max="150" bind:value={buildingCount} /></label>
</div>

<canvas bind:this={canvas} width={WIDTH} height={HEIGHT}></canvas>

<style>
  canvas {
    image-rendering: pixelated;
    border: 2px solid #444;
  }

  .controls {
    display: flex;
    gap: 1rem;
    margin-bottom: 1rem;
    font-family: monospace;
  }

  input {
    width: 200px;
  }
</style>
