<script>
  import { onMount } from "svelte";
  let canvas;

  onMount(async () => {
    const { default: Isomer } = await import("isomer");
    const Point = Isomer.Point;
    const Shape = Isomer.Shape;
    const Color = Isomer.Color;

    const iso = new Isomer(canvas);

    // Une grille de base
    for (let x = 0; x < 10; x++) {
      for (let y = 0; y < 10; y++) {
        iso.add(Shape.Prism(Point(x, y, 0), 1, 1, 0.1), new Color(200, 200, 200));
      }
    }

    // Un cube au centre
    iso.add(Shape.Prism(Point(4, 4, 0), 1, 1, 1), new Color(50, 150, 255));
    iso.add(Shape.Prism(Point(6, 5, 0), 1, 1, 2), new Color(255, 100, 120));

    // Un pilier plus haut
    iso.add(Shape.Prism(Point(2, 2, 0), 1, 1, 3), new Color(180, 255, 100));
  });
</script>

<canvas bind:this={canvas} width="600" height="400"></canvas>

<style>
  canvas {
    border: 2px solid #333;
    background: #f4f4f4;
    display: block;
    margin: 2rem auto;
    image-rendering: pixelated;
  }
</style>
