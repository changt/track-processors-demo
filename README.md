# Background blur demo

Camera background blur in the browser with [@livekit/track-processors](https://github.com/livekit/track-processors-js), using either MediaPipe's selfie segmenter or MODNet on LiteRT.js (WebGPU). No LiveKit room is involved; video never leaves the device.

`models/modnet_352x192_folded_float16.tflite` is [MODNet](https://github.com/ZHKKKe/MODNet) (Apache-2.0, © ZHKKKe), converted from the ONNX export at [Xenova/modnet](https://huggingface.co/Xenova/modnet) to TFLite for a fixed 352×192 input.

This is a disposable static build.
