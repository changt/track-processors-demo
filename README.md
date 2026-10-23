# Background replacement demo

Camera background replacement in the browser with [@livekit/track-processors](https://github.com/livekit/track-processors-js), to compare segmentation models on a phone. No LiveKit room is involved; video never leaves the device.

Pick a model from the menu. **Alpha** composites with each model's person confidence as is, so hair and uncertain regions blend in; **Mask** blurs the mask and cuts it at 0.5, the treatment for a binary mask. The URL keeps both choices (`?model=modnet&mode=mask`). The status line shows frames per second and, for one frame in four, how long the model takes from the frame going in to its mask being finished on the GPU, the same measurement for every model. It also shows how long the model's `segment()` call takes to return, which for MediaPipe is only the time to queue its GPU work.

MediaPipe runs on WebGL; the other models need WebGPU.

| Model | Input | Runtime | File |
| --- | --- | --- | --- |
| MediaPipe selfie segmenter, and its landscape variant | 256×256, 256×144 | MediaPipe Tasks | loaded from Google's model storage |
| [MODNet](https://github.com/ZHKKKe/MODNet) (Apache-2.0, © ZHKKKe), from the ONNX export at [Xenova/modnet](https://huggingface.co/Xenova/modnet) | 352×192 | LiteRT.js | `models/modnet_352x192_folded_float16.tflite` |
| [PP-HumanSegV2-Lite](https://github.com/PaddlePaddle/PaddleSeg/tree/release/2.9/contrib/PP-HumanSeg) portrait (Apache-2.0, © PaddlePaddle Authors) | 256×144 | ONNX Runtime Web | `models/pphs_v2_portrait_256x144.onnx` |
| [PP-HumanSegV1-Lite](https://github.com/PaddlePaddle/PaddleSeg/tree/release/2.9/contrib/PP-HumanSeg) portrait (Apache-2.0, © PaddlePaddle Authors), at Paddle.js's lighter size | 288×160 | ONNX Runtime Web | `models/pphs_v1_portrait_288x160.onnx` |
| [Video human matting](https://modelscope.cn/models/iic/cv_effnetv2_video-human-matting/summary) (Apache-2.0, © Alibaba DAMO Academy) | 256×144 | LiteRT.js | `models/vhm_256x144_float16.tflite` |

The models are converted from their original releases for a fixed input size: the PP-HumanSeg models through paddle2onnx with the output trimmed to the person channel, and the TFLite models through onnx2tf with fp16 weights.

Background photo by Samantha Gades on [Unsplash](https://unsplash.com/photos/BlIhVfXbi9s).

This is a disposable static build.
