# @zxing/library

<p>
  <a href="https://www.npmjs.com/package/@zxing/library">
    <img src="https://img.shields.io/npm/v/@zxing/library.svg" alt="npm version" />
  </a>
  <a href="https://github.com/zxing-js/library/blob/master/LICENSE">
    <img src="https://img.shields.io/npm/l/@zxing/library.svg" alt="MIT License" />
  </a>
  <a href="https://github.com/zxing-js/library/actions">
    <img src="https://github.com/zxing-js/library/workflows/Build/badge.svg" alt="Build Status" />
  </a>
</p>

**@zxing/library** is a TypeScript port of the popular ZXing ("zebra crossing") barcode scanning library. It provides a comprehensive, open-source solution for reading and writing various 1D and 2D barcode formats directly in JavaScript/TypeScript environments, including browsers and Node.js.

## Features

- **Multi-Format Support** - Supports QR Code, Data Matrix, Aztec, PDF417, EAN, UPC, Code 39, Code 93, Code 128, and more
- **TypeScript Native** - Written entirely in TypeScript with full type definitions
- **Browser & Node.js** - Works seamlessly in both browser environments (with camera access) and Node.js for server-side processing
- **file_editor & Reader** - Includes both writers/encoders for generating barcodes and readers for decoding

## Quick Start

### Installation

```bash
npm install @zxing/library
```

### Browser - Scan QR Codes from Camera

```typescript
import { BrowserQRCodeReader } from '@zxing/library';

const codeReader = new BrowserQRCodeReader();

codeReader.decodeFromInputVideoDevice(undefined, 'video')
  .then((result) => {
    console.log('Decoded:', result.text);
  })
  .catch((err) => {
    console.error('Error:', err);
  });
```

### Node.js - Decode from Image

```typescript
import { 
  MultiFormatReader,
  BinaryBitmap,
  RGBLuminanceSource,
  HybridBinarizer,
  DecodeHintType,
  BarcodeFormat
} from '@zxing/library';
import sharp from 'sharp';

async function decodeBarcode(imagePath) {
  const { data, info } = await sharp(imagePath)
    .raw()
    .toBuffer({ resolveWithObject: true });
  
  const luminanceSource = new RGBLuminanceSource(
    new Uint8ClampedArray(data),
    info.width,
    info.height
  );
  
  const binaryBitmap = new BinaryBitmap(
    new HybridBinarizer(luminanceSource)
  );
  
  const hints = new Map();
  hints.set(DecodeHintType.POSSIBLE_FORMATS, [BarcodeFormat.QR_CODE]);
  
  const reader = new MultiFormatReader();
  const result = reader.decode(binaryBitmap, hints);
  
  return result.getText();
}
```

### Generate QR Codes

```typescript
import { QRCodeWriter, BarcodeFormat, EncodeHintType } from '@zxing/library';

const hints = new Map();
hints.set(EncodeHintType.ERROR_CORRECTION, 'H');
hints.set(EncodeHintType.MARGIN, 2);

const writer = new QRCodeWriter();
const bitMatrix = writer.encode(
  'https://example.com',
  BarcodeFormat.QR_CODE,
  200,
  200,
  hints
);

// Convert BitMatrix to image data and render to canvas
```

## Supported Barcode Formats

### 2D Barcodes
- QR Code
- Data Matrix
- Aztec
- PDF 417

### 1D Barcodes
- EAN-8, EAN-13
- UPC-A, UPC-E
- Code 39, Code 93, Code 128
- Codabar
- ITF
- RSS-14

## Documentation

For detailed documentation, visit the [official documentation](https://zxing-js.github.io/library/).

- [Getting Started](https://zxing-js.github.io/library/docs/introduction) - Overview and features
- [Setup Guide](https://zxing-js.github.io/library/docs/setup) - Installation and configuration
- [Architecture](https://zxing-js.github.io/library/docs/architecture) - Internal design and components
- [Usage Examples](https://zxing-js.github.io/library/docs/usage) - Practical code examples
- [API Reference](https://zxing-js.github.io/library/docs/api) - Complete API documentation

## Browser Compatibility

The library uses modern web APIs and requires:
- **MediaDevices API** - For camera access in browser
- **TypedArray support** - `Int32Array`, `Uint8ClampedArray`, etc.
- **BigInt support** - Required for PDF417 decoder

## License

MIT License - see [LICENSE](https://github.com/zxing-js/library/blob/master/LICENSE) for details.

## Links

- [GitHub Repository](https://github.com/zxing-js/library)
- [NPM Package](https://www.npmjs.com/package/@zxing/library)
- [Live Demo](https://zxing-js.github.io/library/)
- [Original Java Library](https://github.com/zxing/zxing)
