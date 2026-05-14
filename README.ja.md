# jsQR-es

[
![npm version](https://badge.fury.io/js/jsqr.svg)
](https://badge.fury.io/js/jsqr)
[
![License: Apache-2.0](https://img.shields.io/badge/License-Apache--2.0-blue.svg)
](https://opensource.org/licenses/Apache-2.0)

ピュアJavaScriptのQRコード読み取りライブラリです。生の画像データを受け取り、その中に含まれるQRコードを検出、抽出、解析します。

このプロジェクトは、オリジナルの[jsQR](https://github.com/cozmo/jsQR)のESモジュール対応版を提供します。

## デモ

- **ライブデモ:** [https://code4fukui.github.io/jsQR-es/](https://code4fukui.github.io/jsQR-es/)
- **QRコードジェネレータ:** [https://taisukef.github.io/qrdots/](https://taisukef.github.io/qrdots/)

## 特徴

- **依存関係なし**: ピュアJavaScriptで、すぐに使用可能。
- **汎用性**: ブラウザ、Node.js、Web Workerで動作。
- **堅牢性**: 回転、傾き、歪みのあるQRコードも読み取り可能。
- **包括的**: すべてのQRコードのデータモード（数字、英数字、バイト、漢字、ECI）をサポート。
- **軽量**: フットプリントが小さく、Webアプリケーションに最適。

## インストール

jsQR-esの利用方法は3通りあります。

### 1. ES Module (CDN)

モダンブラウザやビルドツールを使用する場合、CDNから直接モジュールをインポートできます。

```javascript
import { jsQR } from "https://code4fukui.github.io/jsQR-es/jsQR.js";

// ... jsQRを使用
```

### 2. NPM

このライブラリはnpmの `jsqr` パッケージとして提供されています。

```bash
npm install jsqr --save
```

インストール後、プロジェクト内で `import` または `require` して使用できます。

```javascript
// ES6インポート
import jsQR from "jsqr";

// CommonJSリクエア
const jsQR = require("jsqr");
```

### 3. ブラウザの `<script>` タグ

UMDビルドを直接HTMLファイルに組み込むことも可能です。[`jsQR.js`](https://code4fukui.github.io/jsQR-es/jsQR.js) をダウンロードし、`<script>` タグで読み込んでください。

```html
<script src="./path/to/jsQR.js"></script>
<script>
  // jsQR関数はグローバルスコープで利用可能
  const code = jsQR(imageData, width, height);
  if (code) {
    console.log("Found QR code", code.data);
  }
</script>
```

## APIリファレンス

本ライブラリは単一の関数 `jsQR` をエクスポートします。

### `jsQR(imageData, width, height, options?)`

-   `imageData` (`Uint8ClampedArray`): RGBAピクセルデータの配列。通常は canvas 要素の `ImageData` オブジェクトから取得します。
-   `width` (`number`): 画像の幅。
-   `height` (`number`): 画像の高さ。
-   `options` (`object`, オプション): 追加の設定を含むオブジェクト。
    -   `inversionAttempts` (`string`): `"dontInvert"`, `"onlyInvert"`, `"attemptBoth"` (デフォルト), `"invertFirst"` のいずれか。反転した（例: 黒背景に白）QRコードの処理方法を制御します。

#### 戻り値

QRコードが見つかった場合は `QRCode` オブジェクトを返し、見つからない場合は `null` を返します。

`QRCode` オブジェクトは以下の構造を持ちます:

```typescript
{
  // QRコードからデコードされた文字列データ。
  data: string;

  // QRコードの生のバイナリデータ。
  binaryData: number[];

  // デコードされたデータセグメントの詳細情報を提供するデータチャンクの配列。
  chunks: object[];

  // QRコードのバージョン。
  version: number;

  // 画像内におけるQRコードの位置を示すオブジェクト。
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

### 使用例（ブラウザ）

以下は `jsQR` を使用して `<canvas>` 要素からQRコードをスキャンする完全な例です。

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

    // 例: 画像を読み込んでスキャンする
    const image = new Image();
    image.src = "path/to/your/qrcode.png"; // 画像URLに置き換えてください
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

## ライセンス

Apache-2.0 ライセンスの下で公開されています。
