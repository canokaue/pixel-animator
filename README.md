**pixel-animator** is a library for creating small, colorful, pixel-based animations with JavaScript. It's pretty much the same idea as a [fragment shader](https://en.wikipedia.org/wiki/Shader#Pixel_shaders) from graphics programming, but much slower and maybe more fun to write.

![Rainbow sine wave 256×5×16](examples/1567268372138.gif)

_This guide is a work in progress and only essential features are covered for now_.

## Getting Started

All you need to start animating pixels is an [HTML canvas](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas) and a special kind of function that we'll call `colorize`.

Each pixel in an animation belongs a unique coordinate identified by a `column`, `row` and `frame`. The purpose of the `colorize` function is to take a single coordinate and assign a colored pixel to it. Your function will automatically be run for every coordinate in the animation and the result is rendered on the canvas.

### Example 1: White pixel blinking on a grey background

![White pixel blinking on a grey background 8×8×2](examples/1566165863155.gif)

```javascript
import { render } from "pixel-animator";

function colorize({ column, row, frame }) {
  if (column === 5 && row === 2 && frame % 2 === 0) {
    return 1;
  } else {
    return 0.25;
  }
}

render({ colorize }, document.getElementById("my-canvas"));
```

[[Run this code](https://codesandbox.io/s/bright-green-dot-blinking-on-a-dark-blue-background-9dwjg)]

In this function, each pixel says to itself: _If I'm in the sixth column, third row, and my frame number is even, I should be white. If not, make me dark grey_. The values `1` and `0.25` represent white and dark grey.

The example also includes some code needed to connect the function to a canvas.

---

### Example 2: Green pixel moving across a blue background

![Green pixel moving across a blue background 8×8×8](examples/1566159876842.gif)

```javascript
function colorize({ column, row, frame }) {
  if (column === frame && row === frame) return { green: 1 };
  return { blue: 0.5 };
}
```

[[Run this code](https://codesandbox.io/s/green-dot-moving-on-a-diagonal-path-v6gpt)]

Here each pixel says to itself: _If both my column and row numbers are equal to my frame number, I should be bright green. If not, make me dark blue_. And it just happens to trace a diagonal line as the frame number increases over time.

---

### Example 3: Animated gradient

![Animated Gradient 8×8×8](examples/1566072376597.gif)

```javascript
function colorize({
  // Current pixel coordinates
  column,
  row,
  frame,
  // Overall dimensions for comparison
  columns,
  rows,
  frames,
}) {
  return {
    red: column / columns,
    green: row / rows,
    blue: frame / frames,
  };
}
```

[[Run this code](https://codesandbox.io/s/example-animated-gradient-c0gnl)]

## Dealing with a blurry canvas

You might notice that things don't look so sharp when rendering to your canvas element. The following CSS will stop the browser from smoothing things out as you scale the canvas up:

```css
canvas {
  width: 320px;
  height: 320px;
  image-rendering: pixelated;
}
```
