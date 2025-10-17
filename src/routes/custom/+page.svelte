<script>
  import { onMount } from "svelte";

  let canvas, ctx;
  let seed = "pixelville";
  let buildingCount = 1;
  let rng, noise2d;

  const WIDTH = 512;
  const HEIGHT = 512;
  const PIXEL = 8;

  // Taille demi-tile pour isométrique
  const TILE_WIDTH_HALF = 32;  // largeur demi-tile
  const TILE_HEIGHT_HALF = 16; // hauteur demi-tile

  function isoXY(x, y) {
    return {
      x: (x - y) * TILE_WIDTH_HALF + WIDTH/2,
      y: (x + y) * TILE_HEIGHT_HALF
    };
  }

  // Dessine un bâtiment simple
  function drawBuild1() {
    const points = [
      { x: 256, y: 64 },  // haut
      { x: 64, y: 96 },  // droite
      { x: 32, y: 144 },  // bas
      { x: 224, y: 80 }   // gauche
    ];

    ctx.beginPath();
    ctx.moveTo(points[0].x, points[0].y);
    for (let i = 1; i < points.length; i++) {
      ctx.lineTo(points[i].x, points[i].y);
    }
    ctx.closePath();
    ctx.fillStyle = "#3498db";
    ctx.fill();
    ctx.strokeStyle = "#000";
    ctx.stroke();
  }

  // Dessine la grille isométrique
function drawIsoGrid(cols = 10, rows = 10) {
  ctx.strokeStyle = "#999";
  ctx.lineWidth = 1;
  ctx.font = "10px monospace";
  ctx.fillStyle = "#000";

  for (let x = 0; x <= cols; x++) {
    for (let y = 0; y <= rows; y++) {
      const p = isoXY(x, y);

      // point sur la grille
      ctx.fillRect(p.x - 2, p.y - 2, 4, 4);

      // coordonnées réelles (canvas)
      ctx.fillText(`(${Math.round(p.x)},${Math.round(p.y)})`, p.x + 5, p.y - 5);
    }
  }
}

  function drawCity() {
    if (!ctx) return;
    ctx.clearRect(0, 0, WIDTH, HEIGHT);

    drawIsoGrid(10, 10); // dessine la grille
    drawBuild1();        // dessine le bâtiment
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
