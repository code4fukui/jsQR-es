# jsQR-es

[
![npm version](https://badge.fury.io/js/jsqr.svg)
](https://badge.fury.io/js/jsqr)
[
![License: Apache-2.0](https://img.shields.io/badge/License-Apache--2.0-blue.svg)
](https://opensource.org/licenses/Apache-2.0)

> 日本語のREADMEはこちらです: [README.ja.md](README.ja.md)

A pure JavaScript QR code reading library. This library takes in raw images and will locate, extract, and parse any QR code found within.

This project provides an ES module-compatible version of the original [jsQR](https://github.com/cozmo/jsQR).

## Demo

- **Live Demo:** [https://code4fukui.github.io/jsQR-es/](https://code4fukui.github.io/jsQR-es/)
- **QR Code Generator:** [https://taisukef.github.io/qrdots/](https://taisukef.github.io/qrdots/)

## Features

- **Zero Dependencies**: Pure JavaScript, ready to use.
- **Versatile**: Works in browsers, Node.js, and Web Workers.
- **Robust**: Reads QR codes that are rotated, skewed, or distorted.
- **Comprehensive**: Supports all QR code data modes (Numeric, Alphanumeric, Byte, Kanji, ECI).
- **Lightweight**: Small footprint, ideal for web applications.

## Installation

There are three ways to use jsQR-es:

### 1. ES Module (CDN)

For modern browsers and build tools, you can import the module directly from a CDN.

```javascript
import { jsQR } from "https://code4fukui.github.io/jsQR-es/jsQR.js";

// ... use jsQR
```

### 2. NPM

The library is available on npm under the `jsqr` package.

```bash
npm install jsqr --save
```

Then, you can import or require it in your project:

```javascript
// ES6 import
import jsQR from "jsqr";

// CommonJS require
const jsQR = require("jsqr");
```

### 3. Browser `<script>` Tag

You can include the UMD build directly in your HTML file. Download [`jsQR.js`](https://code4fukui.github.io/jsQR-es/jsQR.js) and include it with a script tag.

```html
<script src="./path/to/jsQR.js"></script>
<script>
  // The jsQR function is available in the global scope
  const code = jsQR(imageData, width, height);
  if (code) {
    console.log("Found QR code", code.data);
  }
</script>
```

## API Reference

The library exports a single function, `jsQR`.

### `jsQR(imageData, width, height, options?)`

-   `imageData` (`Uint8ClampedArray`): An array of RGBA pixel data. This is typically retrieved from a canvas element's `ImageData` object.
-   `width` (`number`): The width of the image.
-   `height` (`number`): The height of the image.
-   `options` (`object`, optional): An object with additional configuration.
    -   `inversionAttempts` (`string`): Can be `"dontInvert"`, `"onlyInvert"`, `"attemptBoth"` (default), or `"invertFirst"`. Controls how the library handles inverted (e.g., white on black) QR codes.

#### Return Value

Returns a `QRCode` object if a QR code is found, otherwise `null`.

The `QRCode` object has the following structure:

```typescript
{
  // The decoded string data from the QR code.
  data: string;

  // The raw binary data from the QR code.
  binaryData: number[];

  // An array of data chunks, providing detailed information about the decoded data segments.
  chunks: object[];

  // The version of the QR code.
  version: number;

  // An object describing the location of the QR code in the image.
  location: {
    topRightCorner: { x: number, y: number };
    topLeftCorner: { x: number, y: number };
    bottomRightCorner: { x: number, y: number };
    bottomLeftCorner: { x: number, y: number };

    topRightFinderPattern: { x: number, y: number };
    topLeftFinderPattern: { x: number, y: number };
    bottomLeftFinderPattern: { x: number, y: number };

    bottomRightAlignmentPattern?: { x: number, y: number };
  };
}
```

### Example Usage (Browser)

Here is a complete example of using `jsQR` to scan a QR code from a `<canvas>` element.

```html
<!DOCTYPE html>
<html>
<body>
  <h1>jsQR-es Demo</h1>
  <canvas id="canvas" style="display: none;"></canvas>
  <div id="output"></div>

  <script type="module">
    import { jsQR } from "https://code4fukui.github.io/jsQR-es/jsQR.js";

    const canvas = document.getElementById("canvas");
    const context = canvas.getContext("2d");
    const outputDiv = document.getElementById("output");

    // Example: Load an image and scan it
    const image = new Image();
    image.src = "path/to/your/qrcode.png"; // Replace with an image URL
    image.onload = () => {
      canvas.width = image.width;
      canvas.height = image.height;
      context.drawImage(image, 0, 0, image.width, image.height);

      const imageData = context.getImageData(0, 0, image.width, image.height);
      const code = jsQR(imageData.data, imageData.width, imageData.height);

      if (code) {
        outputDiv.innerText = `Found QR Code: ${code.data}`;
        console.log("Full QR code object:", code);
      } else {
        outputDiv.innerText = "No QR code found.";
      }
    };
  </script>
</body>
</html>
```

## License

Licensed under the Apache-2.0 license.