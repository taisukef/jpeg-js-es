# jpeg-js

A pure javascript JPEG encoder and decoder for Deno

**NOTE:** this is a _synchronous_ (i.e. CPU-blocking) library that is much slower than native alternatives. If you don't need a _pure javascript_ implementation, consider using async alternatives like [sharp](http://npmjs.com/package/sharp) in node or the [Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) in the browser.

## Example Usage

### Decoding JPEGs

Will decode typed array into a `Uint8Array`;

```js
import { JPEG } from "https://taisukef.github.io/jpeg-js-es/JPEG.js";

const jpegData = Deno.readFileSync('grumpycat.jpg');
const rawImageData = JPEG.decode(jpegData);
console.log(rawImageData);
/*
{ width: 320,
  height: 180,
  data: <Uint8Array 5b 40 29 ff 59 3e 29 ff 54 3c 26 ff 55 3a 27 ff 5a 3e 2f ff 5c 3c 31 ff 58 35 2d ff 5b 36 2f ff 55 35 32 ff 5a 3a 37 ff 54 36 32 ff 4b 32 2c ff 4b 36 ... > }
*/
```

#### Decode Options

| Option               | Description                                                                                                                                                                                       | Default     |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| `colorTransform`     | Transform alternate colorspaces like YCbCr. `undefined` means respect the default behavior encoded in metadata.                                                                                   | `undefined` |
| `useTArray`          | Decode pixels into a typed `Uint8Array` instead of a `Buffer`.                                                                                                                                    | `true`     |
| `formatAsRGBA`       | Decode pixels into RGBA vs. RGB.                                                                                                                                                                  | `true`      |
| `tolerantDecoding`   | Be more tolerant when encountering technically invalid JPEGs.                                                                                                                                     | `true`      |
| `maxResolutionInMP`  | The maximum resolution image that `jpeg-js` should attempt to decode in megapixels. Images larger than this resolution will throw an error instead of decoding.                                   | `100`       |
| `maxMemoryUsageInMB` | The (approximate) maximum memory that `jpeg-js` should allocate while attempting to decode the image in mebibyte. Images requiring more memory than this will throw an error instead of decoding. | `512`       |

### Encoding JPEGs

```js
import { JPEG } from "https://taisukef.github.io/jpeg-js-es/JPEG.js";

const width = 320;
const height = 180;
const frameData = new Uint8Array(width * height * 4);
let i = 0;
while (i < frameData.length) {
  frameData[i++] = 0xff; // red
  frameData[i++] = 0x00; // green
  frameData[i++] = 0x00; // blue
  frameData[i++] = 0xff; // alpha - ignored in JPEGs
}
const rawImageData = {
  data: frameData,
  width: width,
  height: height,
};
const jpegImageData = JPEG.encode(rawImageData, 50);
console.log(jpegImageData);
/*
{
  data: Uint8Array(2224) [
    255, 216, 255, 224,  0,  16,  74,  70,  73,  70,   0,   1,   1,  0,  0,
      1,   0,   1,   0,  0, 255, 219,   0, 132,   0,  16,  11,  12, 14, 12,
     10,  16,  14,  13, 14,  18,  17,  16,  19,  24,  40,  26,  24, 22, 22,
     24,  49,  35,  37, 29,  40,  58,  51,  61,  60,  57,  51,  56, 55, 64,
     72,  92,  78,  64, 68,  87,  69,  55,  56,  80, 109,  81,  87, 95, 98,
    103, 104, 103,  62, 77, 113, 121, 112, 100, 120,  92, 101, 103, 99,  1,
     17,  18,  18,  24, 21,  24,  47,  26,  26,  47,
    ... 2124 more items
  ],
  width: 320,
  height: 180
}
*/
// write to file
Deno.writeFileSync('image.jpg', jpegImageData.data);
```

## License

### Decoding

This library builds on the work of two other JPEG javascript libraries,
namely [jpgjs](https://github.com/notmasteryet/jpgjs) for the decoding
which is licensed under the Apache 2.0 License below:

Copyright 2011 notmasteryet

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

### Encoding

The encoding is based off a port of the JPEG encoder in [as3corelib](https://code.google.com/p/as3corelib/source/browse/trunk/src/com/adobe/images/JPGEncoder.as).

The port to JavaScript was done by by Andreas Ritter, www.bytestrom.eu, 11/2009.

The Adobe License for the encoder is:

**Adobe**

Copyright (c) 2008, Adobe Systems Incorporated
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are
met:

- Redistributions of source code must retain the above copyright notice,
  this list of conditions and the following disclaimer.

- Redistributions in binary form must reproduce the above copyright
  notice, this list of conditions and the following disclaimer in the
  documentation and/or other materials provided with the distribution.

- Neither the name of Adobe Systems Incorporated nor the names of its
  contributors may be used to endorse or promote products derived from
  this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS
IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO,
THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR
CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

## Contributing

jpeg-js is an **OPEN Open Source Project**. This means that:

> Individuals making significant and valuable contributions are given commit-access to the project to contribute as they see fit. This project is more like an open wiki than a standard guarded open source project.

See the [CONTRIBUTING.md](https://github.com/eugeneware/jpeg-js/blob/master/CONTRIBUTING.md) file for more details.

### Contributors

jpeg-js is only possible due to the excellent work of the following contributors:

<table><tbody>
<tr><th align="left">Adobe</th><td><a href="https://github.com/adobe">GitHub/adobe</a></td></tr>
<tr><th align="left">Yury Delendik</th><td><a href="https://github.com/notmasteryet/">GitHub/notmasteryet</a></td></tr>
<tr><th align="left">Eugene Ware</th><td><a href="https://github.com/eugeneware">GitHub/eugeneware</a></td></tr>
<tr><th align="left">Michael Kelly</th><td><a href="https://github.com/mrkelly">GitHub/mrkelly</a></td></tr>
<tr><th align="left">Peter Liljenberg</th><td><a href="https://github.com/petli">GitHub/petli</a></td></tr>
<tr><th align="left">XadillaX</th><td><a href="https://github.com/XadillaX">GitHub/XadillaX</a></td></tr>
<tr><th align="left">strandedcity</th><td><a href="https://github.com/strandedcity">GitHub/strandedcity</a></td></tr>
<tr><th align="left">wmossman</th><td><a href="https://github.com/wmossman">GitHub/wmossman</a></td></tr>
<tr><th align="left">Patrick Hulce</th><td><a href="https://github.com/patrickhulce">GitHub/patrickhulce</a></td></tr>
<tr><th align="left">Ben Wiley</th><td><a href="https://github.com/benwiley4000">GitHub/benwiley4000</a></td></tr>
</tbody></table>
