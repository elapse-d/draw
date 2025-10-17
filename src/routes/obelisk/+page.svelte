<script>
  import { onMount } from 'svelte';
  let canvas;

  onMount(async () => {
    const obelisk = await import('obelisk.js');

    const point = new obelisk.Point(270, 120);
    const pixelView = new obelisk.PixelView(canvas, point);

    // Cube principal
    const mainDim = new obelisk.CubeDimension(120, 200, 60);
    const mainColor = new obelisk.CubeColor().getByHorizontalColor(obelisk.ColorPattern.GRAY);
    const mainCube = new obelisk.Cube(mainDim, mainColor);
    pixelView.renderObject(mainCube);

    // Cube “extrusion” ou petit trou côté droit
    const extrudeDim = new obelisk.CubeDimension(40, 100, 60); // plus petit cube
    const extrudeColor = new obelisk.CubeColor().getByHorizontalColor(obelisk.ColorPattern.DARK_GRAY);
    const extrudeCube = new obelisk.Cube(extrudeDim, extrudeColor);

    // Position de l’extrusion : décalage X vers la droite
    const extrudePoint = new obelisk.Point(point.x + 120, point.y + 50); // ajuster selon effet souhaité
    const extrudeView = new obelisk.PixelView(canvas, extrudePoint);

    extrudeView.renderObject(extrudeCube);
  });
</script>

<canvas bind:this={canvas} width="600" height="400" id="canvas-demo"></canvas>

<style>
  canvas {
    border: 2px solid #333;
    display: block;
    margin: 2rem auto;
    background: #e0e0e0;
    image-rendering: pixelated;
  }
</style>
