<script>
  import { onMount } from "svelte";

  let canvas, ctx;
  const WIDTH = 256;
  const HEIGHT = 256;
  const TILE_WIDTH = 32;  // largeur du cube
  const TILE_HEIGHT = 16; // hauteur du cube

  // Fonction pour transformer les coordonnées 3D en isométrique
  function isoXY(x, y, z) {
    return {
      x: (x - y) * TILE_WIDTH / 2 + WIDTH / 2,
      y: (x + y) * TILE_HEIGHT / 2 - z
    };
  }

  function drawCube(x, y, z, color) {
    const top = isoXY(x, y, z);
    const left = isoXY(x, y + 1, 0);
    const right = isoXY(x + 1, y, 0);

    // Faces du cube
    ctx.fillStyle = color.top;
    ctx.beginPath();
    ctx.moveTo(top.x, top.y);
    ctx.lineTo(right.x, right.y);
    ctx.lineTo(right.x, right.y + TILE_HEIGHT);
    ctx.lineTo(top.x, top.y + TILE_HEIGHT);
    ctx.closePath();
    ctx.fill();

    ctx.fillStyle = color.left;
    ctx.beginPath();
    ctx.moveTo(top.x, top.y);
    ctx.lineTo(left.x, left.y);
    ctx.lineTo(left.x, left.y + TILE_HEIGHT);
    ctx.lineTo(top.x, top.y + TILE_HEIGHT);
    ctx.closePath();
    ctx.fill();

    ctx.fillStyle = color.right;
    ctx.beginPath();
    ctx.moveTo(right.x, right.y);
    ctx.lineTo(left.x, left.y);
    ctx.lineTo(left.x, left.y + TILE_HEIGHT);
    ctx.lineTo(right.x, right.y + TILE_HEIGHT);
    ctx.closePath();
    ctx.fill();
  }

  onMount(() => {
    ctx = canvas.getContext("2d");

    // Dessiner un cube à la position (0,0) et hauteur 64 pixels
    drawCube(0, 0, 64, {
      top: "#88cc88",
      left: "#559955",
      right: "#66aa66"
    });
  });
</script>

<canvas bind:this={canvas} width={WIDTH} height={HEIGHT}></canvas>

<style>
  canvas {
    image-rendering: pixelated;
    border: 2px solid #444;
    background: #fff;
  }
</style>
